# Coderstats

http://coderstats.geeksta.net/

## Example Queries

Number of not forked repos of specified user
db.repos.find({user_id:ObjectId("4f4a3a70b744de2fd90000ff"), fork:false}).count()

Get all users with number of owned repos
db.repos.group({ key: {"user_id": true}, initial: {sum: 0}, reduce: function(doc, prev) {if (doc.fork===false) prev.sum += 1} })

## TODOs

* Add info about Chrome extension
* Achievements: (btn class buttons above Share stats)
* Omit user "type": "Organization" from top coder stats? requires user type in repos collection
* Check for duplicates in users collection by gh_login and remove duplicates
* Make charts embeddable
* Terms of service based on http://en.wordpress.com/tos/
* Fix Github oauth login, put auth data into auth collection
* Fetch and evaluate more data like for authenticated users, e.g.
    * users followers https://api.github.com/users/yaph/followers
    * the last 10 commits for the 10 most recently updated repositories https://api.github.com/repos/yaph/coderstats/commits
    * Alternatively parse users RSS feed
    * Fetch data from coderwall provided user has specified the coderwall name into coderwall collection
    * Allow data update if older than 24 hrs
* delete account feature for authenticated users
* User RSS feed
* Don't display empty tabs
* More details on repos like collaborators http://developer.github.com/v3/repos/collaborators/

## Weight calculation for repo

Think about implications, e.g. user confusion, when weighting repos before 
implementing this. Weighting has to be explained and the explanation must be easy
to find.

* weight = repo * size
  if now - updated_at > six_months
    subtract 10%
  else if now - created_at > two_years
    add 10%

## Mongodb Collections

All fields except for gh_login are optional, use github data if available. Github
specific fields are prefixed with gh_ and automatically updated. All other fields
are set when user is created, i.e. logs in or is requested the first time.

* users
    * gh_login required
    * login
    * name
    * hide_avatar       bool that indicates whether avatar is hidden
    * email             not displayed publicly
    * created_at        when user is requested or logs in the 1st
    * updated_at        when was data from github updated
    * avatar_url
    * homepage
    * location
    * hireable
    * company
    * gh_followers
    * gh_type
    * public_gists
    * gh_following
    * gh_public_repos
    * gh_html_url
    * gh_created_at

* repos
    * user_id   _id from users collection
    * open_issues
    * watchers
    * pushed_at
    * homepage
    * git_url
    * updated_at
    * fork
    * forks
    * language
    * private
    * size
    * clone_url
    * created_at
    * name
    * html_url
    * description

## Other Services to consider
* http://confluence.atlassian.com/display/BITBUCKET
* http://www.ohloh.net/api/getting_started
