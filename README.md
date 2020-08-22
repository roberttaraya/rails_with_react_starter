# README
1. run `mkdir my_new_app && cd my_new_app`
2. rails generator command with options to create this rails api shell for backend:

  `rails new .  --api --skip-test --no-rdoc --no-ri --database=postgresql --skip-keeps --skip-bundle`

3. to add rspec, add the following to Gemfile within development, test groups:

  ```
    %w[rspec-core rspec-expectations rspec-mocks rspec-rails rspec-support].each do |lib|
      gem lib, git: "https://github.com/rspec/#{lib}.git", branch: 'master' # Previously '4-0-dev' or '4-0-maintenance' branch
    end
  ```
4. to add foreman, add `gem 'foreman'` to Gemfile within development group
5. run `bundle install`
6. run `npx create-react-app client`


  > NOTE: If create-react-app had been previously installed globally via `npm install -g create-react-app`, the package should first be uninstalled before running the previous step using `npm uninstall -g create-react-app` to ensure that npx always uses the latest version.


7. change client/package.json to:

  ```
    {
      "name": "client",
      "version": "0.1.0",

      // add this line:
      "proxy": "http://127.0.0.1:3000",

      "private": true,
      "dependencies": {
        "@testing-library/jest-dom": "^4.2.4",
        "@testing-library/react": "^9.3.2",
        "@testing-library/user-event": "^7.1.2",
        "axios": "^0.20.0",
        "react": "^16.13.1",
        "react-dom": "^16.13.1",
        "react-scripts": "3.4.3"
      },
      "scripts": {

        // modify these scripts
        "start": "PORT=3001 react-scripts start",
        "build": "react-scripts build && cp -r build/* ../public/",

        "test": "react-scripts test",
        "eject": "react-scripts eject"
      },
      "eslintConfig": {
        "extends": "react-app"
      },
      "browserslist": {
        "production": [
          ">0.2%",
          "not dead",
          "not op_mini all"
        ],
        "development": [
          "last 1 chrome version",
          "last 1 firefox version",
          "last 1 safari version"
        ]
      }
   }

  ```


8. in the app root dir, create `Procfile.dev`. add the following:

  ```
  web: cd client && yarn start
  api: bundle exec rails s -p 3000

  ```

9. create `start.rake` in cd ./lib/tasks:

  ```
  namespace :start do
    desc 'Start dev server'
    task :development do
      exec 'foreman start -f Procfile.dev'
    end

    desc 'Start production server'
    task :production do
      exec 'NPM_CONFIG_PRODUCTION=true npm run postinstall && foreman start'
    end
  end
  task :start => 'start:development'

  ```

10. run `rails start` in root of app dir
