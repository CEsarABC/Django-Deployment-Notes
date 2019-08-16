## Django Deployment Steps

### Steps to Deploy a Django Walking Skeleton

These steps will help you deploy a Django project with no apps from start to finish.

1. Create an environment

2. Install Django

   - `sudo pip3 install Django==1.11.17`

3. Create your Django Project called Tester

   - `django-admin startproject Tester .`
     - It's important to remember the full stop at the end of this command, otherwise, your project won't run. This command will create your django project folder that contains your `settings.py, wsgi.py` and your top-level `urls.py` file. You will also see a `manage.py` file outside of your Django project folder.

4. Make migrations to create your dbSqlite3 database.

   - `python3 manage.py makemigrations`
     - You should see a response of `No changes detected`, as you haven't created any other apps or models yet.

5. Run migrations on your database

   - `python3 manage.py migrate`
     - You will see migrations applied for Django's build in User Authentication models that will give you the ability to create a super user to access the admin panel.

6. Run your app for the first time.

   - On AWS Cloud 9: `python3 manage.py runserver 0.0.0.0:8080`
   - On GitPod: `python3 manage.py runserver`
   - When you open your app, you will likely see an `ALLOWED_HOSTS` error, saying that you need to add a specific URL to your `ALLOWED_HOSTS` list in your `settings.py` file.

7. Add this URL to your `ALLOWED_HOSTS` in your `settings.py` file.

   - The format should be `ALLOWED_HOSTS = ['YOUR_LOCAL_URL_HERE']`

8. Run your app again.

   - You should see a page that says "Congratulations, it worked!"

9. Install gunicorn to prepare for deployment.

   - `sudo pip3 install gunicorn`

10. Create a Procfile

    - `echo "web: gunicorn YOUR_DJANGO_APP_NAME.wsgi:application" > Procfile`
      - `YOUR_DJANGO_APP_NAME` should be the exact same name as the name of the Django project that you created in step 3. It is case sensitive. Make sure that your Procfile has a capital P and that it follows the format above exactly, otherwise your app won't deploy.

11. Create your requirements.txt file.

    - `sudo pip3 freeze --local > requirements.txt`

12. Initialize a local GitHub repository and view the files that are being tracked by GitHub.

    - `git init`
    - `git status`
    - When you do a `git status` you should see all files that have been created with the Django project, along with your database.

13. Add your `db.sqlite3` database to a `.gitignore` file, as we don't need to add this database to GitHub.
    - `echo "db.sqlite3" > .gitignore`
      - This will create a `.gitignore` file and add the string `"db.sqlite3"` to it. If you do `git status` again, you'll no longer be able to see `"db.sqlite3"`, and you'll now see `.gitignore` instead.
14. Create a repository on GitHub and continue committing to GitHub for the first time.

    - `git add .`
    - `git commit -m "Your commit message"`
    - `git remote add origin YOUR_HTTPS_GITHUB_REPO_LINK`
    - `git push -u origin master`

      - The `git remote add origin` command connects your local Git repository in your workspace to the remote repository that's stored on GitHub. You only need to do these last two commands the first time you push to GitHub.

15. Create a Heroku app.

    - Go to the Heroku dashboard at `https://dashboard.heroku.com/` and create a new app.

16. Connect your Heroku app to your GitHub repository.

    - In the deploy tab on your Heroku Dashboard, you should see a `Deployment Method` section. Here, you can select GitHub, and connect to your GitHub account if you haven't used this deployment method previously.
    - In the App Connected to GitHub section, you'll want to search for your respository and click the Connect button next to it.

17. Add `DISABLE_COLLECTSTATIC` to your Heroku config vars.

    - Since we're not adding static files to our Django app yet, you'll want to to got the settings tab on Heroku, click on the `Reveal Config Vars` button, and add
      - `DISABLE_COLLECTSTATIC` as the Key and `1` as the Value.

18. Deploy your app for the first time.

    - In the deploy tab that you were previously on, scroll down to the Manual Deploy section at the bottom of the page. Make sure the master branch is chosen, and click on `Deploy Branch`. This will trigger a build of your app.

19. Open your app.

    - You will see another `ALLOWED_HOSTS` error that will tell you to add the URL to your Heroku app to your `ALLOWED_HOSTS`. So, you'll want to go back to your `settings.py` file and add this URL.
      - The format should be `ALLOWED_HOSTS = ['YOUR_LOCAL_URL_HERE', 'YOUR_LIVE_URL_HERE']`

20. Add, commit, and push this change to GitHub.

21. Deploy your app again.

    - You'll want to go back to step 18 and repeat the deployment process from the master branch.

22. Congratulations! It worked!
    - If you open your app on Heroku, you should see the `Congratulations! It worked!` page. You now have a full Django Walking Skeleton, and you're ready to start adding apps!
