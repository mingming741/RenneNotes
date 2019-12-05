### GIL
GIL(Global Interpreter Lock)为Python中的脚本执行器。Python的multi-thread其实并不是multi-thread，GIL的本质使得任何时候，多线程的任务，同时只有一个线程在运作（即使CPU是多核）。

Python中每一个process都会有一个GIL，因此python的多线程其实是单线程的轮训。但是如果有的线程被IO卡主的话，那么任务会交给IO，这时候GIL会release给其他线程。说明Python的多线程在IO bound的时候也是有效率提升的。

Python中每个.py 文件都是一个process，一个py文件也可以分支处其他的process，例如`os.system(cmd)`。如果要使用multi process，主process最好写上`if __name__ == __main__`，这样保证主函数只在主线程中执行，而不会被分支出去的自己执行。
