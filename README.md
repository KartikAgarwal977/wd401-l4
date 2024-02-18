# Testing 
In today's software development landscape, continuous testing is crucial for ensuring the reliability and quality of software. This involves initiating testing early in the design phase, continuing throughout the development process, and even extending into production deployment

## Type of Testing
There are various types of testing, including Integration testing, Unit testing, and more. For this project, we focus on Integration and Unit testing, leveraging frameworks such as Cypress and Jest.

## Continuous Integration with GitHub Actions
### GitHub Action Workflow
We have set up a CI/CD pipeline using GitHub Actions to automate the testing and deployment processes.
``` yaml
name: Auto test L10 solution
on: push
env:
  PG_DATABASE: database_test
  PG_USER: postgres
  PG_PASSWORD: kartik
jobs:
  run-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:11.7
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: kartik
          POSTGRES_DB: database_test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
      - name: Check out repository code
        uses: actions/checkout@v3

      - name: Install dependencies
        run: cd todo-app && npm install

      - name: Run unit tests
        run: cd todo-app && npm test --env=test

      - name: Run the app
        id: run-app
        run: |
          cd todo-app
          npm install
          # NODE_ENV=test npx sequelize-cli db:drop
          # NODE_ENV=test npx sequelize-cli db:create
          # NODE_ENV=test npx sequelize-cli db:migrate
          npm run start:prod &
          sleep 5
      - name: Run integration tests
        run: |
          cd todo-app
          npm install cypress cypress-json-results
          npx cypress run --env STUDENT_SUBMISSION_URL="http://localhost:3000"
```
## ScreenShots
Here are some screenshots showcasing the CI/CD pipeline in action:
![image](https://github.com/KartikAgarwal977/wd401-l4/assets/101928227/f864a24e-b8b1-44eb-adc3-c412d652d52b)

![image](https://github.com/KartikAgarwal977/wd401-l4/assets/101928227/418af676-726c-4f65-9dbc-ff406cd0576f)

![image](https://github.com/KartikAgarwal977/wd401-l4/assets/101928227/0a19e1b4-cfa6-4538-8b62-db6461230292)



## unit test
We employ the Jest framework along with Node.js for unit testing. Jest allows us to thoroughly test individual units of the project for various scenarios, ensuring robustness and reliability.
## integrated testing
For integration testing, we utilize the Cypress framework. While I am still gaining proficiency with Cypress, I have included sample codes to verify functionalities such as signup and login. Though the test coverage is currently limited due to time constraints, I am committed to expanding the test suite in the near future.

Please feel free to visit our GitHub repository at <a href="https://github.com/KartikAgarwal977/todo-web-app">todo-web-app </a> for further insights. We are continuously striving to enhance our testing capabilities to deliver high-quality software solutions.

Your feedback and suggestions are always welcome as we aim to maintain professionalism and excellence in our development practices
