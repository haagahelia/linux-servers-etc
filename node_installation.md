
# Node and Node app installation

## Pre-requisite process:

1. Make sure Database server is running
2. Fetch the SWP22S database scripts
3. Run them against the DB server
4. Test the database connection and database data via some PC client, e.g. HeidiSQL

## Process of installing Node and making a node project run 

1. Fetch the SWP22S repo to (this time same) backend server
2. Install node software with apt install
3. Try to run the project with npm start (fails as some settings are missing, .env file. SSH pipe?)
4. Try to do the changes needed
5. Try to rerun again. Blocked by already running version of the backend
6. Possibly test with PostMan or VS Code HttpRequest RestClient

## Node install commands
```
node --version
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt install npm

node --version
npm --version
npx -- version
nodemon --version
```
Now node and related tools should be available? All versions available and recent?

## Fetching node backend project from some public git repo and making it run
```
git clone https://github.com/haagahelia/swp22s-backend.git
pwd
ls -Falls
cd swp22s-backend
npm install
npm start
// you will find out missing .env file and thus app will not start
// ... add the .env file with proper settings
npm start
// you will find out that the ssh pipe to database might be missing
// ... run the statement for creating the pipe
npm start
```

If the application cannot start based on EADDRINUSE error, there
is already a Node running in that port. If so, find out which process and kill it

```
sudo lsof -i | grep node
sudo lsof -i | grep 3000         // Or 8777, or 8787

sudo kill -9 1234                // If the process id was 1234
npm start
```

## Test the app via browser or PostMan or VS Code RestClient
Then open e.g. browser and test some backend endpoint
..or use e.g. PostMan to test them.

## How to get a new version of the Node project if someone has published one?
```
pwd
ls -Falls
// if needed: 
cd swp22s-backend       // Or similar
git pull
npm install             // Someone might have added a new library
npm start
```



