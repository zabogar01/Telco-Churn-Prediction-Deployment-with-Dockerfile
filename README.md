# Telco-Churn-Prediction-Deployment

*This readme is specifically to provide quick instruction on how to run and use the model deployment, not the analysis of the data itself

Before we start building our API, normally we need to export our model using the jupyter file, however for convenience purposes i've already added the model file (model_telco.pkl)

There will be 2 part, first we build the API using Flask App, specifically Flasggr. Then we will also use Dockerfile so that the environment for deployment can be standardized and makes it portable so that everyone can easily use it.


## Creating Flask App for Machine Learning

1. The first step is to import all of the packages, get the csv file ready and read through jupyter

![image](https://user-images.githubusercontent.com/78836385/126109566-c1f928b3-1cd0-439b-96e2-121e559c040c.png)


2. Then, run all of the Telco_Churn file (In my case i use jupyter), or you can just run the code from Machine Learning section and further until you successfully run the code to export the model

![image](https://user-images.githubusercontent.com/78836385/125919382-34abbcd3-5d69-43a3-989d-c87cc47d2f39.png)


3. After that we will be using VSCode from now on (you can use other source-code editor), first thing we will import all of the packages as seen in the image below

![image](https://user-images.githubusercontent.com/78836385/126111265-93aabe42-a074-49f0-b2d1-7555d92da7be.png)


4. To successfully run the app these two codes must be executed to indicate the start and end of the flask app code

![image](https://user-images.githubusercontent.com/78836385/126112045-a9682004-15e9-470d-9df2-9bb0a467765e.png)
![image](https://user-images.githubusercontent.com/78836385/126112097-958bf709-62bf-440e-9b6e-4238f0eeabf5.png)


5. Load the model from the jupyter file that you have exported and then assign it to variable, in this case i assigned it to model_telco
![image](https://user-images.githubusercontent.com/78836385/126112685-b12a830d-86df-4d34-bf1b-f7d69e033440.png)

6. Create a homepage route for your flask app, the default local host should be http://127.0.0.1:5000/, 

![image](https://user-images.githubusercontent.com/78836385/126522641-f3dfffb1-a45d-4a3a-be8c-1ef1bf4f22d6.png)


in this case the homepage should look like this

![image](https://user-images.githubusercontent.com/78836385/126113424-bdb31172-969e-4528-b04b-5041a812444f.png)


7. To start create function for prediction in the flaskapp, we need to create separate route for it as we use a customized template for flask (swagger)

![image](https://user-images.githubusercontent.com/78836385/126114774-8fe33741-c9a4-473e-a15c-01e8e7a06f47.png)


8. Then below is the parameters (features) that will be shown in the UI, the parameters represent the features we have in our data, so in this case we have a total of 19 parameters.

- The 'name' represent the variable name of the feature
- 'in' is the type of variables, because it is a query parameters we will write query in all of the parameters
- 'required' is whether data is mandatory to fill, because we need all of the features, we put all 'required' to be true

![image](https://user-images.githubusercontent.com/78836385/125920557-ab3fae9f-690d-4614-bf70-260cc49c90c2.png)

9. After you run the code you will get message on the terminal like this

![image](https://user-images.githubusercontent.com/78836385/126522889-485bb0c9-6dac-4595-9bc9-30d81a3acf1b.png)


10. Run wherever your local host is and then add /apidocs
or, click : http://127.0.0.1:5000/apidocs/

11. After you successfully run in your browser it should look like this

![image](https://user-images.githubusercontent.com/78836385/126116466-9ca513cd-aedf-4d11-8729-254a7338284b.png)


12. To start predicting, click Try it out on the parameter

![image](https://user-images.githubusercontent.com/78836385/126116740-d28c45e2-9686-4a84-923c-caec9c31620d.png)



11. After that you should be able to input any data (As this is an early build, the value of the data must be exactly the same as the telco_churn original value)

![image](https://user-images.githubusercontent.com/78836385/125922092-da9e89e3-42e6-4c28-b575-07413d3e1159.png)

12. After clicking execute your response/answer wether the customer will churn or not located in the response body

![image](https://user-images.githubusercontent.com/78836385/125922483-faa5ba6e-53c1-443e-8ad6-87701ab3e72c.png)


## Containerization using Dockerfile

After we succseffuly create our API we can start using Dockerfile, make sure that the files name is exactly Dockerfile (without extension)

1. Set the base image, in this case we will be using python
```
FROM python:3.8-slim-buster
```

2. Create working directory which makes Docker to use this path as the default location for all subsequent commands
```
WORKDIR /app
```

3. Copy the files in the directory to image 'app' directory
```
COPY . /app
```

4. Install all of the packages listed in the requirements.txt
```
RUN pip3 install -r requirements.txt
```

5. Expose the application to the specific ports
```
EXPOSE 5000
```

6. Finally we can run our image inside the container using CMD
```
CMD ["python3","app.py"]
```

The full code should be like this

![image](https://user-images.githubusercontent.com/78836385/126528779-94cd3040-70d0-471d-9a4d-87b90b9a1a06.png)


As mentioned above the file should be name as Dockerfile otherwise there might be an error

7. After we create the Dockerfile, using the terminal we can start to build our dockerfile image. However make sure you have the right directory using cd <folder name>
```
docker build --tag docker_app . 
```

8. If it's successful, you can check your docker image using
```
docker images
```
  
9. Now we can start to run the app (and specified the port)
```
docker run -p 5000:5000 docker_app
```
Your terminal should be like this (you can ignore the warning)
  
![image](https://user-images.githubusercontent.com/78836385/126529995-acd45b90-5182-4ca0-97ad-872d6461ea1c.png)

10. You can go to http://127.0.0.1:5000/ to view the homepage or http://127.0.0.1:5000/apidocs/ if you want to predict

  

This this model can predict fairly decent, we can predict more acurate if the customer is not churn than if its churn, however it is normal for an imbalance data but we did manage to have an f1-score of 0.62 on test data so it's better than predicting randomly

![image](https://user-images.githubusercontent.com/78836385/126117413-a39c15d4-8cce-4d2c-9916-a4109ddbdac5.png)
