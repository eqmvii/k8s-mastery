# Curl the local python flask app

curl --header "Content-Type: application/json" --request POST --data '{"sentence":"hello","password":"xyz"}' http://localhost:5050/analyse/sentiment

Flask dependencies not being locked could cause the issue, consider:

 Werkzeug==0.16.1

From https://github.com/pallets/flask/issues/2549

# The python container's ip address

 "IPAddress": "",

# Booting up the Java app pointing to the inspected docker ip of the python ap

docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://172.17.0.2:5000' eqmvii/k8stutorial-java-backend:1
