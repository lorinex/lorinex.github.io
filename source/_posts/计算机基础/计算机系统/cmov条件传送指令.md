---
categories:
  - 计算机基础
  - 计算机系统
---
<h3><a id="cmov_0"></a>cmov条件传送指令</h3> 
<ul><li> <p><strong>指令介绍</strong></p> 
    <pre><code>cmovx 	S,	D
</code></pre> <p><img src="https://img-blog.csdnimg.cn/20201019231806708.png?x-oss-process&#61;image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25wdTIwMTczMDIyODg&#61;,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述" /></p> 
  <ol><li>条件传送指令集每条指令都有两个操作数&#xff1a;源寄存器或者内存地址S&#xff0c;和目的寄存器R&#xff0c;这些指令的结果取决于条件码的值。<strong>源值可以从内存或者源寄存器中读取&#xff0c;但是只有在指定的条件满足时&#xff0c;才会被复制到目的寄存器中</strong>。</li><li><strong>源和目的的值可以是16位、32位或64位长</strong>&#xff0c;不支持单字节的条件传送&#xff0c;汇编器可以从目标寄存器地名字推断出条件传送指令的操作数长度。&#xff08;这点与无条件的指令有差别&#xff0c;因为无条件指令的操作数显式地编码在指令名中&#xff0c;如movw、movl&#xff09;</li></ol> </li><li> <p><strong>Example</strong></p> 
    <pre><code class="prism language-code">cmp   %rsi,%rdi
cmova %rdx,%rax	
</code></pre> 
    <p>这个代码片段的含义为&#xff1a;</p> <p>如果RDI寄存器中的值大于RSI寄存器中的值&#xff0c;则把RAX寄存器中的值替换为RDX寄存器中的值。</p> </li><li> <p><strong>为什么使用条件传送指令会提高性能&#xff1f;</strong></p> <p><strong>当机器遇到条件跳转指令的时候</strong>&#xff0c;一般都会使用分支预测方法来预测哪个分支会被执行&#xff0c;从而使流水线充满指令&#xff0c;提高效率&#xff1b;但是<strong>分支预测不会总是正确的</strong>&#xff08;特别是对于一些诸如x&lt;y不可预测的条件&#xff09;&#xff0c;如果错误预测的话&#xff0c;就需要处理器丢掉之前的工作&#xff0c;从正确位置处的指令开始去填充流水线&#xff0c;这就会<strong>浪费很多时钟周期&#xff0c;导致程序性能下降</strong>。</p> <p><strong>而使用条件传送指令</strong>&#xff0c;就可以取消“处理器分支预测”这一步&#xff0c;<strong>减少控制冒险。</strong></p> <p>可以配合下面这个例子进行理解&#xff1a;</p> <p><img src="https://img-blog.csdnimg.cn/20201019231824528.png?x-oss-process&#61;image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25wdTIwMTczMDIyODg&#61;,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述" /></p> </li><li> <p><strong>条件传送指令不会提高性能的情况</strong></p> <p>不过因为条件传送指令需要一开始就将两种情况的结果计算出来&#xff0c;当不需要被执行的分支计算量很大时&#xff0c;这样做显然是不划算的。</p> <p>在《深入理解计算机系统》这本书中&#xff0c;提到&#xff1a;</p> 
  <blockquote> 
   <p><strong>我们对GCC的实验表明&#xff0c;只有当两个表达式都很容易计算时</strong>&#xff0c;例如表达式分别都只是一条加法指令&#xff0c;<strong>它才会使用条件传送</strong>。根据我们的经验&#xff0c;即使许多分支预测错误的开销会超过更复杂的计算&#xff0c;GCC还是会使用条件控制转移。</p> 
  </blockquote> <p>我认为这就可能是老师所说的“GCC优化保守”的原因。甚至对于那些有着高分支预测错误率的benchmark&#xff08;如541leela_r、505mcf_r、557xz_r&#xff09;如果能更多地使用条件传送指令&#xff0c;可能会进一步提高性能&#xff08;这只是我的一个猜想&#xff0c;具体还是要根据这些benchmark的源码来下定论&#xff09;。</p> </li><li> <p><strong>不适用条件传送指令的情况</strong></p> 
    <p>由于条件传送指令需要将两种情况的结果计算出来&#xff0c;这就有可能会导致问题。如&#xff1a;
    </p> <pre><code class="prism language-c"><span class="token keyword">int</span> <span class="token function">cread</span><span class="token punctuation">(</span><span class="token keyword">int</span><span class="token operator">*</span> xp<span class="token punctuation">)</span>
<span class="token punctuation">{<!-- --></span>
    <span class="token keyword">return</span> <span class="token punctuation">(</span>xp<span class="token operator">?</span> <span class="token operator">*</span>xp<span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre> <p>使用条件传送指令时&#xff0c;会分别将“*xp”和“0”放入寄存器&#xff0c;如果xp为NULL时&#xff0c;就会出现空指针引用错误。因此在做这方面优化的时候&#xff0c;一定要注意&#xff0c;两种情况的任意一个都不能出现错误或者副作用。</p> </li><li> <p><strong>补充&#xff1a;如何确定分支预测错误的处罚</strong></p> <p><img src="https://img-blog.csdnimg.cn/20201019231845202.png#pic_center" alt="在这里插入图片描述" /></p> </li></ul>