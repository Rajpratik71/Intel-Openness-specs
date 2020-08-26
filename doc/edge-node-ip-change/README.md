**OPENNESS**

**DYNAMIC EDGE NODE IP CHANGE FEATURE**

**Working:**

We have created a gRPC service &#39;_newIP&#39;_ that will be called as soon as the edge node IP changes. As the IP of the edge node is changed, RPC newIP will be called in the controller which will update the node IP in Node GRPC Targets table of the Controller CE database of the controller and also register the changed IP of the node to the proxy.

**Scenario Flow Step by Step:**

1. GoLang script will continuously monitor the edge node IP

2. If IP Changed?  ---NO---> Continue Running

3. If Yes, Create a transport credentials object that will load the credentials from the certs directory that will authenticate the newIP grpc request made by the node in the controller_

4. Call the grpc.DialContext method with controller grpc endpoint and credentials passed as parameters that will return a client connection object

5. Update the new IP of the edge node in the Node GRPC target table of the Controller CE database.

6. Register the changed IP of the node to the proxy to inform the proxy that the controller is serving this new IP

7. Call the NewIP method implemented in the controller with the new IP and client connection object passed as parameter.

**Tests Performed:**

**1. Check if node&#39;s details are still present in the controller UI after edge node IP changes.**

STATUS: **Passed**

The edge node details can still be found in the controller UI after edge node IP change.

**2. Check the node interfaces details in the controller UI after edge node IP change.**

STATUS : Passed

The node interfaces details can be found in the controller UI even after edge node IP change.

**3. Check the status of deployed applications after the edge node IP changes.**

STATUS :Passed

The status of the deployed application remains the same after the edge node IP changes.

**4. Deploy a new application after the edge node IP changes.**

STATUS : Passed

Deployed an another application after the edge node IP changes and faced no issued deploying applications after IP change.

**5. Check for the docker logs in the appliance container of the node.**

STATUS: Passed

**Checked for the docker logs after edge node IP change and the node was able to communicate with the controller.**

1. We made changes in the server.go file in grpc directory of the controller. So to build the new edgecontroller repository, we had to change the YAML files in the openness experience kits.
2. We have Added _NewIP_ method in server.go implemented by gRPC server interface of the controller.
3. We have made changes in server.go file of grpc directory in edgecontroller. Build the cce container with updated code.
4. Run the GoLang script &quot;ip\_update\_client.go&quot; as a background process in edgenode by using _go run ip\_update\_client.go &amp;_.
5. IP will be updated in edge controller database.
