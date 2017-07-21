# Directions for deploying to DreamHost
1. Clone [this repo](https://github.com/irenfro/irenfro.github.io/) to the website root
2. Move the __assets__ directory and the __robots.txt__, __humans.txt__ files inside of the *public* directory on the server
3. Clone the repo that has your resume to the website root: For example [my resume repo](https://github.com/irenfro/resume)
  
   * Make a directory called *resume* inside of the *public* directory on the server
   * Link the __resume.pdf__ file, that is in the repo, into the *resume* directory you just made
   * For example: `ln path/to/resume.pdf path/to/public/resume`
4. Remove __index.html__ and rename __real_index.html__ to __index.html__

   * The current __index.html__ page is just a redirect to my actual website

#### The Rest is assuming that you are using `python-flask` and `passenger` to host the app

5. Move the __index.html__, __projects.html__, and __404.html__ to the *templates* directory
6. Put the template path into the Flask app
7. Run the server

#### Here is a sample passenger_wsgi.py file
```python
import sys, os
INTERP = os.path.join(os.environ['HOME'], 'bin', 'python') # leads to the python interpreter
template_path = os.path.join(os.environ['HOME'], 'templates') # leads to the templates folder
if sys.executable != INTERP:
	os.execl(INTERP, INTERP, *sys.argv)
sys.path.append(os.getcwd())

from flask import Flask, render_template
application = Flask(__name__, template_folder=template_path)

@application.route('/')
def root():
	return render_template('index.html')

@application.route('/projects.html')
def projects():
	return render_template('projects.html')
```
