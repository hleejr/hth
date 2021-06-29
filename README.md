# hth

# 📜 Lab 12: Deployments

### ⏱ Agenda

1. 📖 **Guided Tour**: Deploy Tutorial on Heroku](#40m--guided-tour-deploy-tutorial-on-heroku)
   1. [Step 0: Before We Get Started (5m)](#step-0-before-we-get-started-5m)
   2. [Step 1: Create Procfile (5m)](#step-1-create-procfile-5m)
   3. [Step 2: Add Dependencies to requirements.txt (5m)](#step-2-add-dependencies-to-requirementstxt-5m)
   4. [Step 3: Create New Heroku App (5m)](#step-3-create-new-heroku-app-5m)
   5. [Step 4: Setup Static Root (5m)](#step-4-setup-static-root-5m)
   6. [Step 5: Push to Heroku (5m)](#step-5-push-to-heroku-5m)
   7. [Step 6: Push to GitHub (2m)](#step-6-push-to-github-2m)
   8. [Step 7: Release Early and Often (5m)](#step-7-release-early-and-often-5m)

## 🏆 Objectives

1. Demonstrate a Django deployment illustrated live, step by step, with guided mini activities and skills drills.
2. Develop a generalized, standardized plan for deploying Django projects.
3. Describe each step required to deploy a Django project to a remote server.


📖 **Guided Tour**: Deploy Tutorial on Heroku

### Step 0: Before We Get Started (5m)

**This guide is resource meant to walk you through deploying ANY Django project on Heroku. Please note the following guidelines to ensure your deployment experience goes smoothly**!

1. Replace references to `uniqueprojectname` with a unique and memorable name for your project.
     - **Spaces aren't allowed in a project name, so be sure to replace any spaces in the string with dashes (`-`)**.
2. Replace references to `projectname` with the name of your Django project.
     - **Not sure what the project's name is?** Locate the folder that contains `settings.py` ‒ the name of that folder is your Django project name.

Now that we know how to use this guide later on, let's get started. **Open your your Django tutorial codebase in your editor**, and complete each step as guided by the instructor.

### Step 1: Create Procfile (5m)

In your project root, **create a file called `Procfile`** and **add the following line of code** inside:

```txt
web: gunicorn projectname.wsgi --log-file -
```

**What does this line of code tell Heroku to do?**
- `web`: Informs the Heroku Dyno that your project is a web project.
- `projectname.wsgi` Tells the Dyno to look at a file called `wsgi` in the folder called `projectname`.
    - This file is included when you create a new Django project.
- `—-log-file -` Write all terminal output to the logs.
    - View your Heroku logs by running `heroku logs`.

### Step 2: Add Dependencies to requirements.txt (5m)

Heroku recommends completing this step early on to make sure your `Procfile` works, and that the Dyno has everything it needs in order to successfully install your project and deploy it live.

🤔 **What files are required to successfully deploy on Heroku?**

The following files in this list live in the project's root directory, and **must be included in order to successfully deploy your project**:

- `requirements.txt`: A list of dependencies to install before starting the server.
- `Procfile`: Contains the command Heroku will run to start the server.

Add the new dependencies to `requirements.txt` so that they are installed when you next push to Heroku:

```txt
django==2.2.7
gunicorn
```

### Step 3: Create New Heroku App (5m)

- **3a**: In your terminal, run the following command to create a new Dyno, where your project will be installed and deployed for use by the public.

    ```bash
    heroku create uniqueprojectname
    ```

- **3b**: Open your editor, and add the following strings to the blank `ALLOWED_HOSTS` list in `settings.py`:

    ```python
    ALLOWED_HOSTS = ['localhost', 'uniqueprojectname.herokuapp.com']
    ```

    🤔 **What do these values mean?**
    - `localhost`: Development environment ‒ the place where you write code.
    - `uniqueprojectname.herokuapp.com`: Production environment - the place you deploy your finished code to show others.

### Step 4: Setup Static Root (5m)

Add the following string to `settings.py`:

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

### Step 5: Push to Heroku (5m)

Add, commit, and push your changes to Heroku!

```bash
git add .
git commit -m "tweaked settings for deployment"
git push heroku master
```

#### Common Issues & Questions

- ⚠️ **GET AN ERROR?**: When deploying with Git, Heroku wants to initiate the repo from the project root, where your ` manage.py`  file lives. You might notice in the tutorial repo that `manage.py` is one level down. Even though running the app locally with Heroku worked, if you try to deploy with `git push heroku master` you’ll receive an error that says `Failed to detect app`. You can fix this by running the following command instead of `git push heroku master`:

    ```bash
    git subtree push --prefix projectname heroku master
    ```

- 🛑 **GET A DIFFERENT ERROR?**: Run `heroku logs` in your terminal, then raise your hand to receive 1 on 1 feedback from your instructor on the state of your deployment.
- 🤔 **Why does pushing to Heroku take so long?!**
  - Each time you push to Heroku, it kicks off the following deployment workflow:
    1. Examine `requirements.txt` for changes since the last deployment.
          - Installs new dependencies automatically.
    2. Run all new migrations found since the last deployment.
          - Heroku executes the exact same `manage.py` commands we've practiced all term!
    3. Launch the production server according to the command in the `Procfile`.

### Step 6: Push to GitHub (2m)

Once you know everything works, push to GitHub.

```bash
git push origin master
```

### Step 7: Release Early and Often (5m)

**So far, we've accomplished**:

- Creating a live website, located at `https://uniqueprojectname.herokuapp.com`.
- We've done all the work required to release new features and bug fixes early and often!

<p style="color:red; font-weight: bold;">You should strive to frequently push to your production server ‒ anytime you fix a bug or add a new feature. That means you may find yourself pushing code to Heroku multiple times a day ‒ just like most major companies!</p>
