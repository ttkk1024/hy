==========
快速入门
==========

.. image:: _static/cuddles-transparent-small.png
   :alt: Karen Rustard's Cuddles

(Thanks to Karen Rustad for Cuddles!)


**如何快速学习Hy**:

1. 创建 `虚拟Python环境
   <https://pypi.python.org/pypi/virtualenv>`_.
2. 激活虚拟Python环境.
3. 从 `GitHub <https://github.com/hylang/hy>`_ 安装Hy ``$ pip install git+https://github.com/hylang/hy.git``.
4. 启动 ``hy`` REPL.
5. 在REPL输入::

       => (print "Hy!")
       Hy!
       => (defn salutationsnm [name] (print (+ "Hy " name "!")))
       => (salutationsnm "YourName")
       Hy YourName!

       etc

6. 完成后输入CTRL + D .

*太不可思议了！这样的程序让人兴奋不已！我想写Hy程序.*

7. 打开优秀的编程编辑器然后输入代码::

       #! /usr/bin/env hy
       (print "I was going to code in Python syntax, but then I got Hy.")

8. 保存为 ``awesome.hy``.
9. 令它可执行::

        chmod +x awesome.hy

10. 并运行你的首个Hy程序::

        ./awesome.hy

11. 做个深呼吸以免过度压抑.
12. 会心一笑并偷偷溜进你的橱柜，做一些不可描述的事情.
