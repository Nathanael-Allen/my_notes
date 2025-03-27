To start I have a script that "builds" the application out of my typescript source code and tailwind. In the package.json file:
```json
{
    "build-tw": "npx @tailwindcss/cli -i ./src/styles/input.css -o ./src/styles/output.css",
    "copy-assets": "node copyAssets.js",
	"build": "npm run build-tw && npm run copy-assets && tsc"    
}
```
Running `npm run build` will compile my source code and copy all files into my `dist` folder. Then I copy the `dist` folder into a docker image with a Dockerfile
```yml
FROM node:lts
WORKDIR /app
COPY package.json package.json
COPY package-lock.json package-lock.json
RUN npm install --omit=dev
COPY dist .
CMD ["node", "server.js"]
```
Once the image is built using 
```
docker build -t application-name .
```
you can tag it and link it to a repository on docker hub
```
docker tag <image-name>:<version> nathanaela1/<repo-name>:<version>
docker push nathanaela1/<repo-name>:<version>
```
After the image is pushed to the repo I can pull it down into my VPS using
```
sudo docker pull nathanaela1/<repo-name>:latest
```
Then build it
```
sudo docker build -p 3000:3000 -d --restart always --name <conatainer-name> <image-name>
```
