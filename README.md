Sourcegraph programming challenge
=================================

## Instructions

1. Make an HTTP service that satisfies the requirements below. If anything is ambiguous, please first refer to the test script provided. Otherwise,
feel free to shoot us an email.
2. Test your service using the script provided (`test.sh`). Start your server and then follow the instructions in the script to run it against your
service. We will test your service with a different script and also on tens of thousands of names, so please write code that generalizes and scales.
3. Create a README that contains the following:
   * For each API endpoint, state its space and runtime complexities (Big O) and explain why your implementation achieves these. If you considered
   alternate implementations, explain why you chose the right one. Be clear and concise (no more than a few sentences per endpoint).
   * Include build instructions that tell us how to compile and run your program. You are welcome to use external libraries in your solution, but
   make sure they are covered by your build instructions.
4. Create a zip/tar of your solution directory and email to hiring@sourcegraph.com with the subject line, "challenge solution".

## Requirements

Create a web service that annotates HTML snippets by hyperlinking names. Names satisfy the following regex: `[A-Za-z0-9]+`. ("Bob09" is an example
name. The string "Alex.com" contains 2 names: "Alex" and "com".)

The service should expose an HTTP API that supports the following operations:

1. Create/update the link for a particular name using an HTTP `PUT` on a URL of the form `/names/[name]`. The body of the request contains JSON of the
form `{ "url": "[url goes here]" }`.
2. Fetch the information for a given name using an HTTP `GET` on a URL of the form `/names/[name]`. This should return JSON in the following format:
`{ "name": "[name goes here]", "url": "[url goes here]" }`
3. Delete all the data on an HTTP `DELETE` on the URL `/names`. (Note: data is NOT required to persist between server restarts.)
4. On an HTTP `POST` to the URL `/annotate` where the request body is an HTML snippet, return an annotated snippet with hyperlinks on all occurrences
of names stored in the server. Given the input snippet, this method should find all instances of currently stored names in the input snippet. It
should wrap each name instance in a hyperlink (`<a href="...`) to the corresponding URL. It should do this ONLY for text that is not currently
hyperlinked. It should not modify any tag names or attributes. Assume the input HTML is well-formed HTML on one line; the returned HTML should not
have any newlines or extraneous spaces. You should only annotate complete names that are not part of a larger name. For example, if your server
contains the names "Alex" (`http://alex.com`) and "Bo" (`http://bo.com`) and the input snippet is `Alex Alexander <a href="http://foo.com"
data-Bo="Bo">Some sentence about Bo</a>`, then the expected output is `<a href="http://alex.com">Alex</a> Alexander <a href="http://foo.com"
data-Bo="Bo">Some sentence about Bo</a>`. See the test script for further examples.

5. Your implementation should scale to storing millions of names and annotating snippets that are as long as a typical webpage (e.g., `nytimes.com`).
All API endpoints at this scale should run in near-real-time (i.e., at most a few seconds).
6. For any endpoint that mutates state, the following contract should hold: after a client receives a response, the change should be reflected in all
subsequent API calls. E.g., if I have completed a `PUT` of a new name, I should immediately be able to `GET` it.

## Contents of this directory
- `README.md` is this README.
- `test.sh` is the test script you should run to test your server.
- `expected_out.txt` is the expected output that the test script compares against. You should run the test script in the same directory that contains `expected_out.txt`.

## Other guidelines

All implementation details are up to you. Feel free to use whatever language or external libraries you like. You are welcome to use any tools or
resources at your disposal (e.g., Sourcegraph, documentation, GitHub, Google Search, etc.), but the work must be your own – no collaboration with
others.

We estimate that this challenge should take roughly 1 to 3 hours to complete. Feel free to reach out to us with any questions, and happy coding!