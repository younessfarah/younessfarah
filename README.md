#!/bin/bash

### ======================================================================= ###
###     A nagios plugin to check la limite d'execution des apex asynchrone  ###
###     Uses: ./ScriptApexLimits.sh /                                       ###
###                                                                         ###
###                                                                         ###
### ======================================================================= ###

GetToken="$(curl https://login.salesforce.com/services/oauth2/token -d 'grant_type=password' -d 'client_id=3MVG9sh10GGnD4Dvq41Theuh9gDn2IN1FyzjXPYOTxP46dVx_08qmnEyDoKCOQQ5Dy9QXQu2ghAqlVV85_0LB' -d 'client_secret=48D3821F66C5AB6AD359DD2B08234CE37DA82A6DB12045FE0DABCA7DDC4364C5' -d 'username=prod.salesforceB2C@inwi.ma.inwib2c' -d 'password=xxxxxxxxxx'| jq .access_token | tr -d '"')"

DailyAsyncApex=$(curl https://inwi-b2c.my.salesforce.com/services/data/v53.0/limits/ -H 'Authorization: Bearer '.$GetToken -H 'X-PrettyPrint:1'| jq '.DailyAsyncApexExecutions .Remaining')

if [[ $DailyAsyncApex -lt "79999" ]] 

then

  echo "CRITICAL - 80% d'execution des apex asynchrone utilisé "
  exit 3

else

  echo "OK"
  exit 0
fi

