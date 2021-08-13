# AlexaIntentFunctions

INSTALL THIS PACKAGE INSIDE THE LAMBDA FOLDER

This is a file containing the functions used for running APL / Standard card / speakoutput / and a random repromt. This uses lists defined at the top of the page.
this requires a list called displayList.js which will contain name,title,URL, description,speakOutput, document, datasources.
the document and datasources should be linked to the file containing the apldocument and apldatasources respecivly.
the apl files should also be defined at the top of your list file so that it can gather the information.

        let responseBuilder = intentMethod.canDoAPLFunction(handlerInput, *insertIntentNameHere*);
        const speakOutput = intentMethod.speakOutputFunction(intentName);
        const repromptText = intentMethod.repromptFunction();

        return responseBuilder.speak(speakOutput).reprompt(repromptText).getResponse()
      
this is the code that calls the functions inside this file.
the intentName should be the same as the name: used in your created list, this is how the functions link the intent with the correct information stored in the list.
then the intent name should be passed into the functions to allow them to access the variable created.

        function APLDataSettings(intentData){

        intentData.datasources.detailImageRightData.image.sources[0].url = `${intentData.URL}`;
        intentData.datasources.detailImageRightData.textContent.locationText.text = `${intentData.description}`;
        intentData.datasources.detailImageRightData.textContent.primaryText.text = `${intentData.title}`;
        };
  
This is the code that changes the data inside the APL.
after the = and the intentData.datasources will get the information from inside the list you have created.
the middle section should be changed to match the APL you are using so that the code will change the correct information.
This should be created inside lambda/helpers/helper.js

        const repromptList = [
            { description: 'What else would you like to know?'},
            { description: 'We have trusties, volunteers, and a team helping us out all the time. If you would like to get to know them more.'}
            ]
        module.exports = { repromptList};

This is the reprompt list the key needs to be known as description. This needs to be created in lambda/models/repromptList.js

        const displayList = [
            { 
                name: 'LAUNCH', 
                title: 'Welcome to BASIS', 
                URL: 'http://www.basissouthessex.org.uk/wp-content/uploads/2015/01/SmallLogo-1-150x150.jpg',
                background: 'https://i.imgur.com/Ov9XAaR.jpg', 
                description: 'Welcome to BASIS feel free to ask questions such as tell me more, what activities do we run or how you can get in contact with us',
                speakOutput: 'Welcome to BASIS feel free to ask questions such as tell me more, what activities do we run or how you can get in contact with us',
                document:aplDocument,
                datasources:aplDataSource
            } 
        ]
        module.exports = { displayList};

This is the displayList which contains all the data on all the intents that you have. this is what the functions look through to get any information that needs to be displayed.
this also gets created as a lambda/models/displayList.js file. At the top of this file is where you will need to call your apldocument and apldatascource

        function voiceWrap(stringToWrap) {
            return `<voice name="Amy"><lang xml:lang="en-GB">${stringToWrap}</lang></voice>`;
        }

This is the voiceWrap Function which is what you will change to be able to customise the voice that speaks in the speakoutput and reprompt. Any specific word changes will
have to be done inside the displaylist, speakoutput string as this will change the settings for the whole string. This will be created inside the lambda/helpers/helper.js file.