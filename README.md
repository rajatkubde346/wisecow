1.Clone the repository:

git clone https://github.com/nyrahul/wisecow

2. write a dockerfile(dockerfile):

FROM node:14
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]

3.Build the Docker image:

docker build -t wisecow-app .

4.Test the Docker image locally: 

docker run -p 3000:3000 wisecow-app 

Kubernetes Deployment

1.Create Deployment(deployment.yaml):

apiVersion: apps/v1
kind: Deployment
metadata:
  name: wisecow-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wisecow
  template:
    metadata:
      labels:
        app: wisecow
    spec:
      containers:
        - name: wisecow-container
          image: wisecow-app:latest
          ports:
            - containerPort: 5000

2.Create Service YAML(service.yaml):

apiVersion: v1
kind: Service
metadata:
  name: wisecow-service
spec:
  selector:
    app: wisecow
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: LoadBalancer

3.Apply the manifest files:

kubectl apply -f deployment.yaml

kubectl apply -f service.yaml    
