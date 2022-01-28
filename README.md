# python-github-actions-example

![](https://github.com/nikhilkumarsingh/python-github-actions-example/workflows/Python%20application/badge.svg)

Example for creating a simple CI/CD pipeline for a Python Project using Github Actions.

[Watch Tutorial Video Here](https://youtu.be/WTofttoD2xg)

Now, You know how to create a Continous Integration with the help of workflow. But you had to deploy your application so that end user can see it.
So to set a CI / CD where CI has already been configured, we had to set CD for the application. For that, Go to Heroku --> Download and Install Heroku CLI.
Heroku CLI: https://devcenter.heroku.com/articles/heroku-cli 
Once done with the installation, you will get the commands to be run on the documentation page itself. 
Go to your systems cmd, run commands:
  heroku create

![image](https://user-images.githubusercontent.com/25689468/151544327-e425b2c2-ea0f-4f61-8932-5532e948b80d.png)
![image](https://user-images.githubusercontent.com/25689468/151544503-f3c8102f-c2f7-48ba-93d9-63d055f1ae98.png)
This is the default page, but you need to deploy your application over here:
![image](https://user-images.githubusercontent.com/25689468/151544550-002c8122-0ca9-4438-85b9-83bd4eaa8de9.png)
Now, we want to deploy our updates on the above link. I can't do "git push heroku master" everytime. I want it to be automatically done when the build is passed ie., when the greencheck is available on the git hub repository.
For that I had to make a new Github action in order to deploy the code to heroku.
Also, in workflow we are using ubuntu machine which is unaware of the heroku credentials and whenever there be the automated build I cant provide my heroku credentials everytime. For that I'll create a token. For that follow the below steps:
Go to Documentation Page of Heroku --> Deployment --> Heroku CLI Commands --> heroku authorizations:create --> Run the command
![image](https://user-images.githubusercontent.com/25689468/151546800-fb7a9251-e876-4a79-80bb-2ca54ac9a098.png)
I'll now put this Token in my GitHub repository which can then be used for authentication purpose. I can put this token in my workflow file as well but since my respoistory is public I'll save it somewhere else. Also If you don't want to showcase your application name you can do that as well with the below steps.
To add, follow the below steps:
Go to GitHub --> Settings --> Secrets --> Actions --> New Repository Secret --> 
![image](https://user-images.githubusercontent.com/25689468/151550148-b9805cd5-8328-4edd-971b-b8f335eed00d.png)
For API Name, https://desolate-atoll-01695.herokuapp.com/, use desolate-atoll-01695 in the HEROKU_API_NAME.
Now, put the following code in your workflow:
    - name: Deploy to Heroku
      env:
        HEROKU_API_KEY: ${{ secrets.HEROKU_API_KEY }}
        HEROKU_APP_NAME: ${{ secrets.HEROKU_APP_NAME }}
        if: github.ref == 'refs/heads/master' && job.status == 'success'
        run: |
          git remote add heroku https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git
          git push heroku HEAD:master -f
   
