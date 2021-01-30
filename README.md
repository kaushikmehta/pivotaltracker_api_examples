# my notes
    - make sure you're running own version of ruby on mac
        - ruby v3 doesn't work!
    - add dotenv gem
    - add env file
    - load dotenv (use script)
    - bundle install
    - gem install bundler

# api_examples

This repository contains example scripts showing how to use Tracker's API to get information not readily available via the Tracker application.

The Pivotal Tracker v5 API is documented here -- http://www.pivotaltracker.com/help/api/rest/v5

reportCycleTime.rb:

This script uses the activity endpoint -  https://www.pivotaltracker.com/help/api/rest/v5#projects_project_id_activity_get - to retrieve all the activity for the specified project. (Note that only the most recent six months of activity are available, and it’s returned in reverse order, the most recent activity first). The response to this endpoint is paginated (https://www.pivotaltracker.com/help/api#Paginating_List_Responses), so our example shows how to page through all the entries, 100 items at a time. The script loops through all the activity events, looking for story state changes. It stores the first started date and last accepted at date for each accepted story, using the information returned in the activity resource (https://www.pivotaltracker.com/help/api/rest/v5#activity_resource).

Next, the script shows how to look up the story name and type, then drop release type stories, plus all stories that don’t have both a started at and accepted at date. The remaining stories are sorted by cycle time in ascending order, and a report printed out with the story id, cycle time, and story name (since story names can be long, we’ve ellipsified anything after 40 characters). 

To run the script, set environment variables for your Tracker API TOKEN (https://www.pivotaltracker.com/help/api#Basics) and PROJECT_ID. Depending on the size of your project, it can take a few minutes to run. 

 * Note that if any stories that were accepted in your project were subsequently deleted or moved to another project, the report will show “deleted” instead of the story name. 
 * Since you can only retrieve the last six months of activity, the script can’t calculate cycle time for stories that were started more than six months ago. 
 * It is possible in Tracker to put a story directly into a different active state than ‘started’, for example, a story could go directly from the unstarted to the finished state. This script won’t compute cycle times for those, but of course, you can write your own script to accommodate those differences.

reportCycleTimeAndRejectedCount.rb:

The same script, but it also counts up the number of times each accepted story was rejected, and prints that out along with the cycle time.