pythonnet - Python for .NET
===========================

|Join the chat at https://gitter.im/pythonnet/pythonnet|

|appveyor shield| |travis shield| |codecov shield|

|license shield| |pypi package version| |python supported shield|
|stackexchange shield|

Python for .NET 是一个包，它可以让 Python 程序员几乎无缝集成 .NET 公共语言
运行时 (CLR)，并为 .NET 开发者提供一个功能强大的应用程序脚本工具。它允许 
Python 代码与 CLR 交互，也可以用于将 Python 嵌入到 .NET 应用程序中。

在 Python 中调用 .NET 代码
-----------------------------

Python for .NET 允许将 CLR 命名空间当作真正的 Python 包。

.. code-block::

   import clr
   from System import String
   from System.Collections import *

要加载程序集，请使用 ``clr`` 模块中的 ``AddReference`` 函数:

.. code-block::

   import clr
   clr.AddReference("System.Windows.Forms")
   from System.Windows.Forms import Form

在 .NET 中嵌入 Python
------------------------

-  所有对 python 的调用应该被包含在一个 
   ``using (Py.GIL()) {/* Your code here */}`` 块中。
-  使用 ``dynamic mod = Py.Import("mod")`` 导入 python 模块，然后你就可以
   像平常一样调用函数，例如 ``mod.func(args)``。
-  使用 ``mod.func(args, Py.kw("keywordargname", keywordargvalue))`` 或
   ``mod.func(args, keywordargname: keywordargvalue)`` 来应用关键字参数。
-  所有 python 对象都应该被声明为 ``dynamic`` 类型。
-  涉及 python 和字面值/托管类型的数学操作必须先使用 python 对象，例如，
   ``np.pi * 2`` 有效，``2 * np.pi`` 不行。

示例
~~~~~~~

.. code-block:: csharp

   static void Main(string[] args)
   {
       using (Py.GIL())
       {
           dynamic np = Py.Import("numpy");
           Console.WriteLine(np.cos(np.pi * 2));

           dynamic sin = np.sin;
           Console.WriteLine(sin(5));

           double c = np.cos(5) + sin(5);
           Console.WriteLine(c);

           dynamic a = np.array(new List<float> { 1, 2, 3 });
           Console.WriteLine(a.dtype);

           dynamic b = np.array(new List<float> { 6, 5, 4 }, dtype: np.int32);
           Console.WriteLine(b.dtype);

           Console.WriteLine(a * b);
           Console.ReadKey();
       }
   }

输出:

.. code::

   1.0
   -0.958924274663
   -0.6752620892
   float64
   int32
   [  6.  10.  12.]

关于安装、常见问题、故障排除、调试和使用 pythonnet 项目的信息，可以在 Wiki 中找到:

https://github.com/lidanger/pythonnet/wiki

.. |Join the chat at https://gitter.im/pythonnet/pythonnet| image:: https://badges.gitter.im/pythonnet/pythonnet.svg
   :target: https://gitter.im/pythonnet/pythonnet?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
.. |appveyor shield| image:: https://img.shields.io/appveyor/ci/pythonnet/pythonnet/master.svg?label=AppVeyor
   :target: https://ci.appveyor.com/project/pythonnet/pythonnet/branch/master
.. |travis shield| image:: https://img.shields.io/travis/pythonnet/pythonnet/master.svg?label=Travis
   :target: https://travis-ci.org/pythonnet/pythonnet
.. |codecov shield| image:: https://img.shields.io/codecov/c/github/pythonnet/pythonnet/master.svg?label=Codecov
   :target: https://codecov.io/github/pythonnet/pythonnet
.. |license shield| image:: https://img.shields.io/badge/license-MIT-blue.svg?maxAge=3600
   :target: ./LICENSE
.. |pypi package version| image:: https://img.shields.io/pypi/v/pythonnet.svg
   :target: https://pypi.python.org/pypi/pythonnet
.. |python supported shield| image:: https://img.shields.io/pypi/pyversions/pythonnet.svg
   :target: https://pypi.python.org/pypi/pythonnet
.. |stackexchange shield| image:: https://img.shields.io/badge/StackOverflow-python.net-blue.svg
   :target: http://stackoverflow.com/questions/tagged/python.net
