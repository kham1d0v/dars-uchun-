// joke-generator.js  

const fetch = require('node-fetch');

/**  
 * Function to fetch a random joke from the JokeAPI.  
 * @returns {Promise<string>} A promise that resolves to a joke string.  
 */  
async function fetchJoke() {
    try {
        const response = await fetch('https://v2.jokeapi.dev/joke/Any');
        const data = await response.json();
        if (data.flag) {
            // If joke is a single part
            return data.joke;
        } else {
            // If joke is a two part setup and delivery
            return `${data.setup} - ${data.delivery}`;
        }
    } catch (error) {
        console.error('Error fetching joke:', error);
        return 'Failed to fetch joke';
    }
}

// Example usage
fetchJoke().then(joke => console.log(joke));

