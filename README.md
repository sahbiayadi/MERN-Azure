# mern-stack-azure-deployment-example
This code was based off code written for the for the [MERN Tutorial](https://www.mongodb.com/languages/mern-stack-tutorial). It has been edited however to be deployable as one single app in Azure App Service.

Differences:
1. The server Express app now includes a line `app.use(express.static('public'))` so static files for the UI are fetched from a public folder
2. The front-end now references / instead of localhost:<port>
3. There is a new GitHub Actions workflow file called main_mongodbatlasazurenodejs.yml that will copy the files generated by the client build into the public folder in the server. 

## How To Run locally
Create an Atlas URI connection parameter in `mern/server/config.env` with your Atlas URI:
```
ATLAS_URI=mongodb+srv://<username>:<password>@sandbox.jadwj.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
PORT=5000
```

Build client to generate static files:
```
cd mern/client
npm install
npm run build
```

Copy generated files to public folder in Server: 
```
cp -r ./build ../server/public
```

Start server:
```
cd mern/server
npm install
npm start
```

## How To Deploy to Azure
A more in depth article and video are coming soon.

But a brief overview:
1. Fork this repo.
2. Create a new Azure App Service.
3. In Application Settings for your new App Service, add an application setting (not connection string) called ATLAS_URI and add your Atlas URI value.
4. In Deployment Center, point it at your forked GitHub repo. If it asks to create a yml file, click yes/accept.
5. Update your newly created yml file that is likely to be named main_nameofyourazureappservice.yml so the steps before deploy match the contents of main_mongodbatlasazurenodejs.yml from this repo. This has the steps to install npm on both the server and client, build the client to generate the static files then copy them to the public folder of the server for single app deployment. If you accidentally include the deploy step, it will fail because it won't be pointing at your Azure deployment ID.
6. Updating this file in GitHub will automatically trigger a build and deploy in GitHub Actions. Once deployed, your website will automatically be up and running.

## Disclaimer

Use at your own risk; not a supported MongoDB product.
