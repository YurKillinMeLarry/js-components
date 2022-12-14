#### HTML Structure

- section.wiki
  - div.container
    - img
    - h3(text)
    - form.form
      - input.form-input type='text'
      - button.submit-btn (search) type='submit'
  - div.results
    - div.articles
      - a
        - h4
        - p (lorem20)

#### API DOCS

- [wiki docs](https://www.mediawiki.org/wiki/API:Main_page)

- ready to go url's

#### Initial Setup

- select form, input, results
- listen for submit events
- if empty value, display error
- create fetchPages()
- pass valid input value into the fetchPages()

#### Fetch Pages

- display loading while fetching
- construct dynamic url
- display if error
- display error no items
- create renderResults()
- pass valid results into renderResults()

#### Render Results

- iterate over the list
- pull out title, snippet, pageid
- setup a card
- set results with div.articles and list inside

const url =
'https://en.wikipedia.org/w/api.php?action=query&list=search&srlimit=20&format=json&origin=*&srsearch=';

const formDOM = document.querySelector('.form');
const inputDOM = document.querySelector('.form-input');
const resultsDOM = document.querySelector('.results');

formDOM.addEventListener('submit', (e) => {
e.preventDefault();
const value = inputDOM.value;
if (!value) {
resultsDOM.innerHTML =
'<div class="error"> please enter valid search term</div>';
return;
}
fetchPages(value);
});

const fetchPages = async (searchValue) => {
resultsDOM.innerHTML = '<div class="loading"></div>';
try {
const response = await fetch(`${url}${searchValue}`);
const data = await response.json();
const results = data.query.search;
if (results.length < 1) {
resultsDOM.innerHTML =
'<div class="error">no matching results. Please try again</div>';
return;
}
renderResults(results);
} catch (error) {
resultsDOM.innerHTML = '<div class="error"> there was an error...</div>';
}
};

const renderResults = (list) => {
const cardsList = list
.map((item) => {
const { title, snippet, pageid } = item;
return `<a href=http://en.wikipedia.org/?curid=${pageid} target="_blank"> <h4>${title}</h4> <p> ${snippet} </p> </a>`;
})
.join('');
resultsDOM.innerHTML = `<div class="articles"> ${cardsList} </div>`;
};

const url =
'https://en.wikipedia.org/w/api.php?action=query&list=search&srlimit=20&format=json&origin=*&srsearch=searchValue';

// list=search - perform a full text search
// srsearch="inputValue" - search for page titles or content matching this value.
// srlimit=20 How many total pages to return.
// format=json json response
// "origin=\*" fix cors errors

const page_url = 'href=http://en.wikipedia.org/?curid=${pageid}';
