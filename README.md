# Github Repository List

This build utilizes GitHub's API to grab a list of repositories, and then from there a list of commits from each of the repositories. 

Demo can be accessed [on aws](http://github-repository-app.s3-website-us-east-1.amazonaws.com/).

## Features 

* List view of the 30 most recent repositories from the company (Netflix is the default) with repository name, description, language used, how many stars and forks, and the created date
* 'See more' button to load another 30 using the pagination option of the API
* Input a new company name to see their repositories
* Click on a single repository information and see a list of commits with the commit message, who made the commit, the commit hash, and created date
* List is automatically sorted by most stars, but can be changed to view most forks, language, alphabetical by name, and most recent
* Can switch between list and tile layouts based on user preference 