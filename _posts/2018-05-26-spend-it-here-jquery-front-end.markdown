---
layout: post
title:      "spend-it-here-jquery-front-end"
date:       2018-05-26 17:22:28 -0400
permalink:  spend-it-here-jquery-front-end
---


Coming into this project, I was extremely excited to learn in a trial-by-fire fashion how to implement jQuery into my project. It was invigorating to see firsthand how JavaScript adds dynamic funcationality to an otherwise clunky user experience containing ample page refreshing. Furthermore, I'm mesmerized by the fact that applications provide APIs for other developers to build projects incorporating their data. API endpoints are absolutely fascinating. I was struck with wonder: I thought to myself that I'd definitely be scouring the internet exploring what's out there for a lifetime to come.

Then, came the decision fatigue. As enthused as I was about implementing AJAX and refactoring my controllers to render JSON I had to decide where and for what features was AJAX more appropriate than a page refresh. I thought about making an entirely dynamic application that had no page refreshes, but that seemed a little overboard. Afterall, I want to be able to know how to navigate to a particular business and review. I wanted the user to have that option without re-rendering the business index or relying on a 'back' link.

In the end, I decided to add a commenting feature for users to leave comments on other users' reviews. The form for submitting a comment is rendered dynamically onto the page, after submission--and if it's a valid comment--I make a `POST` request via `$.ajax` where the comment data is then persisted to the database. In order to load all the comments for a particular review, I make a `GET` request to my nested resource `/businesses/:biz_id/reviews/:review_id/comments.json`.

This is where it gets really fun.

Recently, a fellow coursemate and I have been tackling the fun challenge that is algorithm practice. We've been going through exercises and solutions together, wherein one of our problems was to create an array of subchunks from a larger array of data. To illustrate this problem:

```
// Write a function that takes in an array of data and separates them into a given size of smaller chunks.

const array = [boat, terrace, island, jungle, sand, 1, 2, 3, magic, canyons, coyote, 4, 5, 6]

function subChunks(arr, n) {
  const chunks = [];
	let subchunk;
	
  for (let i = 0; i < arr.length; i+= size) {
	  subchunk = arr.slice(i, i + size);
	  chunks.push(subchunk);
	}
	
	return chunks;
}

subChunks(array, 3)  //=> [[boat, terrace, island], [jungle, sand, 1], [2, 3, magic], [canyons, coyote, 4], [5, 6]]

```

I found the application of this solutions to be useful in subchunking my array of JSON response data for review comments. I then appended that data onto the page in sets of 3. Every time the user clicks "Load Comments," three more comments will be appended onto the DOM.

Here's the snippet of this prototype function in action:

![load-comments](https://imgur.com/ynSwck8)
[load-comments](https://i.imgur.com/ynSwck8.gifv)

And, here's the code:

```

Comment.prototype.loadAllComments = function (clicker) {
  let businessId = $('#comment-section').data("business-id");
  let reviewId = $('#comment-section').data("review-id");
  let user; // Need to grab username
  let html;
  let data = $.ajax({
    method: "GET",
    url: `/businesses/${businessId}/reviews/${reviewId}/comments.json`,
    dataType: "json"
  }).done(function (resp) {
    const respChunks = [];
    let subchunk;
    let size = 3;

    if (resp.length) {
      for (let i = 0; i < resp.length; i += size) {
        subchunk = resp.slice(i, i + size);
        respChunks.push(subchunk);
      }
      for (let el of respChunks[clicker]) {
        // debugger;
        $.get(`/users/${el["user_id"]}.json`, function (data) {
          user = data["email"];
          html = `
          <div id="comment-${el.id}">
          <li>${el.content}</li>
          <p>posted by: ${user}</p>
          </div><br>
          `
          $('#comment-section').append(html);
        });
      };
      clicker++;
    } else if (clicker === 0){
      $('#comment-section').append(`
        <h3 id="no-comments">This review does not have any comments.</h3>
        `);
      clicker++;
    }
  }); // End of .done
}// End of Comment.prototype.loadAllComments

```


