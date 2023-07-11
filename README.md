
![example branch parameter](https://github.com/farhandroid/CounterApp/actions/workflows/CI_CD.yml/badge.svg?branch=master)
![Docker](https://img.shields.io/badge/-Docker-orange)  ![Docker](https://img.shields.io/badge/%20-Redux-blueviolet)

![Docker](https://img.shields.io/badge/-React-red) 


# CounterApp

[See more details about this in Medium](https://farhan-tanvir.medium.com/ci-cd-from-github-to-aws-ec2-using-github-action-e18b621c0507)


## To run in dev environment

### `docker-compose up -d --build`
Open [http://localhost:3001](http://localhost:3001) to view it in the browser.

## To run in production environment

### `docker-compose -f docker-compose.prod.yml up -d --build`
Open [http://localhost:8899](http://localhost:8899) to view it in the browser.









name: Deploy to EC2
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Install dependencies
        run: echo "No dependencies to install"
 
      - name: Deploy in EC2
        env:
            PRIVATE_KEY: ${{ secrets.EC2_PRIVATE_KEY  }}
            HOSTNAME : ${{ secrets.EC2_HOST  }}
            USER_NAME : ${{ secrets.EC2_USERNAME  }}
            
        run: |
          echo "$PRIVATE_KEY" > private_key && chmod 600 private_key
          ssh -o StrictHostKeyChecking=no -i private_key ${USER_NAME}@${HOSTNAME} '
          
            #Now we have got the access of EC2 and we will start the deploy .

      - name: Trigger web server restart
        run: systemctl restart nginx






