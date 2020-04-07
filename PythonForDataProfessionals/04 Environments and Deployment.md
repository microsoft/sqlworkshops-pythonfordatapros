![](graphics/solutions-microsoft-logo-small.png)

# Python for Data Professionals

## 04 Environments and Deployment

<p style="border-bottom: 1px solid lightgrey;"></p>

<dl>
  <dt>Course Outline</dt>
  <dt>1 - Overview and Course Setup</dt>
  <dt>2 - Programming Basics</dt>
  <dt>3 Working with Data</dt>
  <dt>4 Deployment and Environments <i>(This section)</i></dt>
    <dd>4.1 Conda</dd>
    <dd>4.2 Pickling</dd>
    <dd>4.3 SQL Server MAchine Learning Services</dd>
<dl>

<p style="border-bottom: 1px solid lightgrey;"></p>

The main installation of Python - sometimes called "Core" or "base" - has a set of parameters it works with. Since it runs on many operating systems, these variables are set and altered in different ways. Here are the primary environment settings on the standard installation of Python:

- PYTHONPATH - Sets the location for the Python interpreter to locate the module files imported into a program.
- PYTHONHOME - The alternative module search path. 
- PYTHONSTARTUP - The initialization file path ( `.pythonrc.py` ) containing the Python source code. It is executed every time you start the interpreter.
- PYTHONCASEOK - For the Windows OS, find the first case-insensitive match in an "import" statement.

You can show all of the variables by importing the base configuration system library, and then calling a print statement:

`import sysconfig`

`sysconfig.get_config_vars()`

If you want to see just one variable, remember, it's just an array:

`sysconfig.get_config_var('LIBDIR')`

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/cortanalogo.png"><b>4.1 pip and Conda</b></p>

To install new packages, you can build the source code manually, but that's not the way it's most often done. Typically you use a "package manager", and the most popular is "pip". The pip program installs and configures most of the libraries you will need for the base installation of Python.

You probably already have the pip program. However, to install pip, you can use the [cURL](https://curl.haxx.se/download.html) program to get it:

`curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py`

Then use Python to run the script to install it:

`python get-pip.py`

From there, you can query the packages you have with this command, from the command-line in your operating system:

`pip list`

You can install a package using this command:

`pip install SomePackage            # latest version`

`pip install SomePackage==1.0.4     # specific version`

`pip install 'SomePackage>=1.0.4'     # minimum version`

And you can remove a package with this command:

`pip uninstall SomePackage`

There is a lot more that you can do with pip, and you can find out the list here:

`pip`

A more robust package manager, which even installs a distribution of Python for you along with other tools, is [Conda](https://conda.io/docs/user-guide/getting-started.html). For this course, you have installed Python using Conda, which not only has a package manager, but also isolates environments for you. This means that you can create a "boundary" of variables, package directories, and more around a name you specify. You can then switch to that environment to create your code, and that code will always have a consistent set of variables and packages.

To create a Conda environment, issue the following command:

`conda create --name`

For instance, this command creates a new environment called "bucktest" and installs the biology package called biopython:

`conda create --name bucktest biopython`

To see the environments, issue the following command:

`conda info --envs`

The one with the asterisk (*) is the one you are using now. To switch to another environment, issue the following command:

`activate bucktest` (In Windows)

`source activate bucktest` (Mac and Linux)

And to see information about that environment, issue the following command:

`conda list`

or just `conda` to find out everything you can do with Conda.

To install packages in that environment, use this command:

`conda install biopython`

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/aml-logo.png"><b> Activity - pip and Conda</b></p>

Now open the `/code/04_EnvironmentsAndDeployment.py` file and follow the instructions you see there for 4.1.

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/cortanalogo.png"><b>4.2 Pickling</b></p>

"Pickling" in Python means to serialize a Python object. Perhaps that isn't very helpful - what it really means is to take the output of whatever you did in Python and make it available again in another environment or program. It's a way of saving the "state" of a program so that it can be transferred and then re-loaded.

It's best illustrated with some code:

`import pickle`

`a = ['1','2','3']`

`PickleFileName = "picklefile"`

`FileObject = open(PickleFileName,'wb')`

`pickle.dump(a,FileObject)`

`fileObject.close()`

Now you can copy that file to a new computer, open Python, and work with it again as if you ran it there:

`import pickle`

`PickleFileName = "picklefile"`

`FileObject = open(PickleFileName,'r') ` 

`b = pickle.load(FileObject) ` 

`b`

And now *a* equals *b*. Of course, your program would be much longer, most often a series of steps, which might for instance do a Machine Learning prediction. 

You can read a lot more about pickling here: https://wiki.python.org/moin/UsingPickle

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/aml-logo.png"><b> Activity - Pickle</b></p>

Now open the `/code/04_EnvironmentsAndDeployment.py` file and follow the instructions you see there for step 4.2.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/cortanalogo.png"><b>4.3 Docker and Flask</b></p>

Two other abstraction levels are useful to think about. You're probably familiar with Virtual Machines - which uses software to emulate hardware. This lets you install a complete new "computer" in a computer's OS. One level up from that abstraction layer is a *Container*. A Container goes slightly further by including a very small kernel of an operating system (most often Linux) to operate a runtime - like Python. This provides an even more consistent environment for your application, since it can also include settings and programs above the Python level. 

The *Flask* micro-framework for Python isn't technically an abstraction layer, it has more to do with serving your application up to a Web call. You'll often see Docker and Flask used together, so you'll cover it here for completeness. Once again, seeing some code is useful to understand - this example comes from the documentation site:

<pre>

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

</pre>

You can probably follow the layout of this code, but there are some specifics here. First, the code imported Flask itself. Next, the code creates an instance of a Flask app, called "app" in this case. From there, the route was set to the base URL call - just as in the main part of a web page. And finally, a simple function returns the words "Hello World!".

So far, nothing is happening - the code is just on disk. However, you can "deploy" the code on a system that is running with these commands (in Linux):

<pre>
$ export FLASK_APP=hello.py
$ flask run
 * Running on http://127.0.0.1:5000/
</pre>

OK...so what? Well, in this case, you could open a Web Browser on that system and type in that URL - and you'll see "Hello World!" pop up on the screen. Of course, real applications are much more complicated, can take POST and GET operations, and much more. But this is a very convenient way to serve up your Python application without having to tell your users to install and run Python.

Of course, there's a lot more to both of these topics - read the references below to learn more.

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/cortanalogo.png"><b>4.3 Operationalizing Python in SQL Server Machine Learning Services</b></p>

SQL Server (2017 and higher) has a mechanism to run Python code by calling it in a Stored Procedure, which can work with a Pickle file or by running SQL Server code directly. The Python is run side-by-side with SQL Server, so as not to allow Python to interfere with SQL Server base processes. This Python extension is part of the SQL Server Machine Learning Services add-on to the relational database engine. It adds a Python execution environment, an Anaconda distribution with the Python 3.5 runtime and interpreter, standard libraries and tools, and the Microsoft product libraries for Python: [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) for analytics at scale and [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) for machine learning algorithms. Python runs in a separate process from SQL Server, to guarantee that database operations are not compromised.

When you run Python "inside" SQL Server, you must encapsulate the Python script inside a special stored procedure, [sp_execute_external_script](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql?view=sql-server-ver15). Here's an example of Python code running in SQL Server using a Stored Procedure:

<pre>
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
'
</pre>



After the script has been embedded in the stored procedure, any application that can make a stored procedure call can initiate execution of the Python code. From there, SQL Server manages code execution in this process:

1. A request for the Python runtime is indicated by the parameter @language='Python' passed to the stored procedure. SQL Server sends this request to the launchpad service. In Linux, SQL uses a launchpadd service to communicate with a separate launchpad process for each user. See the Extensibility architecture diagram for details.
2. The launchpad service starts the appropriate launcher; in this case, PythonLauncher.
3. PythonLauncher starts the external Python35 process.
4. BxlServer coordinates with the Python runtime to manage exchanges of data, and storage of working results.
5. SQL Satellite manages communications about related tasks and processes with SQL Server.
6. BxlServer uses SQL Satellite to communicate status and results to SQL Server.
7. SQL Server gets results and closes related tasks and processes.

You can see that process here: 

<br>
<img src="https://docs.microsoft.com/en-us/sql/advanced-analytics/python/media/remote-sqlcc-from-python3.png?view=sql-server-2017">
<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/aml-logo.png"><b> Activity - Run Python in a SQL Server Stored Procedure</b></p>

 - Ensure you have [the pre-requisites completed for the installation of SQL Server Machine Learning Services](https://docs.microsoft.com/en-us/sql/machine-learning/install/sql-machine-learning-services-windows-install?view=sql-server-ver15) installed.
 - [Open this reference and follow the steps you see there](https://docs.microsoft.com/en-us/sql/machine-learning/tutorials/quickstart-python-create-script?view=sql-server-ver15).

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="./graphics/thinking.jpg"><b>For Further Study</b></p>

- [You can learn more about Docker here](https://www.fullstackpython.com/docker.html)
- [More on Flask](http://flask.pocoo.org/)
- [Creating a simple Flask application](http://containertutorials.com/docker-compose/flask-simple-app.html)
- [More on SQL Server Machine Learning Services is here](https://docs.microsoft.com/en-us/sql/machine-learning/what-is-sql-server-machine-learning?view=sql-server-ver15)

Congratulations! You now know the basics or working with Python and Data. As you can see, there's a lot more to learn - so use your new knowledge to expand on what you have learned. 