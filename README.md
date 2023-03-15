![RealEstatePricePrediction](https://user-images.githubusercontent.com/87910771/225391185-1d4a0ec7-0ba9-4ebb-90ce-914bf65dada4.png)


<p align="center">
  <h2 align="center">REAL ESTATE PRICE PREDICTION</h2>

  <p align="center">
    This Project is a Real Estate Price Prediction using Machine Learning.
  </p>
</p>

![](BHP_website.PNG)

This data science project series walks through step by step process of how to build a real estate price prediction website. We will first build a model using sklearn and linear regression using banglore home prices dataset from kaggle.com. Second step would be to write a python flask server that uses the saved model to serve http requests. Third component is the website built in html, css and javascript that allows user to enter home square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price. During model building we will cover almost all data science concepts such as data load and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, gridsearchcv for hyperparameter tunning, k fold cross validation etc. Technology and tools wise this project covers,

1. Python
2. Numpy and Pandas for data cleaning
3. Matplotlib for data visualization
4. Sklearn for model building
5. Jupyter notebook, visual studio code and pycharm as IDE
6. Python flask for http server
7. HTML/CSS/Javascript for UI
8. AWS EC2 for cloud deployment

# Deploy this app to cloud (AWS EC2)

1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic.
2. Create a Key Pair and save in C:\Users\Username\\.ssh\sample.pem. This will be used to connect to EC.
3. Now connect to your instance using a command like this, Paste the code found in your ec2 instance
    ```
    ssh -i "C:\Users\<Username>\.ssh\sample.pem" ubuntu@ec2-3-94-185-6.compute-1.amazonaws.com.amazonaws.com
    ```
4. Once you connect to EC2 instance you can clone this repo using this command,
    ```
    git clone https://github.com/AnishLohiya/aws-ml-deploy.git
    ```

5. nginx setup
   1. Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   2. Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   3. Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   4. Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.


6.  After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,
    1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
        listen 80;
            server_name main;
            root /home/ubuntu/aws-ml-deploy/client;
            index index.html;
            location /api/ {
                rewrite ^/api(.*) $1 break;
                proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/main.conf
    ```
    3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restart nginx,
    ```
    sudo service nginx restart
    ```
7. Now install python packages and start flask server
    ```
    sudo apt-get install python3-pip
    sudo pip3 install -r /home/ubuntu/aws-ml-deploy/requirements.txt
    python3 /home/ubuntu/aws-ml-deploy/client/server.py
    ```
  Running last command above will prompt that server is running on port 5000.

8. Now just load your cloud url in browser (for me it was http://ec2-3-94-185-6.compute-1.amazonaws.com/) and this will be fully functional website running in production cloud environment




---
The dataset used in this project is from Kaggle which is the [Bengaluru House Price Data](https://www.kaggle.com/amitabhajoy/bengaluru-house-price-data).

---

# License

[MIT License](https://github.com/AnishLohiya/aws-ml-deploy/blob/master/LICENSE)

# Contributing Guidlines

Please refer to [CODE_OF_CONDUCT.md](https://github.com/AnishLohiya/aws-ml-deploy/blob/master/CODE_OF_CONDUCT.md) and [CONTRIBUTING.md](https://github.com/AnishLohiya/aws-ml-deploy/blob/master/CONTRIBUTING.md) before contributing.
