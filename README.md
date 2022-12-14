# Election_Analysis_Challenge

## Overview
This challenged analyzes the file [Election_results.csv](https://github.com/heatherhutchinson211/election_analysis_challenge/blob/main/election_results.csv) of the votes from three different counties, for the different political candidates. 

### Purpose
The purpose of this analysis was to present some vital information from the polls.  These results include:
* the total number of votes
* the voter turnout form each county
* the count and percentage of votes from each county
* the county with the largest turnout
* the count and percentage of votes each candidate receieved
* the winning candidate
The results were then written to a text file for others to read clearly.

## Results
This was the code I used to analyze the election_results.csv file:
```
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}


# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county = ""
largest_county_vote = 0


# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:

            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] +=1


# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county in county_votes:
        # 6b: Retrieve the county vote count.
        county_vote_count = county_votes[county]
        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = int(county_vote_count) / int(total_votes) * 100

         # 6d: Print the county results to the terminal.
        county_results = (
            f"{county}: {county_vote_percentage:.1f}% ({county_vote_count:,})\n"
        )
        print(county_results, end="")
         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote_count > largest_county_vote):
            largest_county_vote = county_vote_count
            largest_county = county

    # 7: Print the county with the largest turnout to the terminal.
    winning_county = (
        f"--------------\n"
        f"The largest county was: {largest_county}\n"
        f"--------------\n"
    )
    print(winning_county)
    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county)
    
    # Save the final candidate vote count to the text file.

    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
```

* After running this code, I found that the total number of votes received was 369,711
* Of the three counties, Jefferson had 10.5% of the votes with a vote count of 38,855.  Denver county had 82.8% with 306,055 votes.  Finally, Arapahoe county had 6.7% of votes, with 24,801 votes total.
* This made Denver county the county with the highest amount of votes. 
* As far as candidate results, Charles Casper Stockham received 85,213 votes which accounted for 23.0% of total votes.  Diana DeGette received 272,892 votes which was 73.8% of all votes.  Finally, Raymon Anthony Doane had 11,606 votes which made up 3.1% of teh total votes. 
* Diana DeGette was the winner of this election. 

These results can be viewed in the text file: [election_analysis.txt](https://github.com/heatherhutchinson211/election_analysis_challenge/blob/main/election_analysis.txt)

<img width="298" alt="Screenshot 2022-12-04 at 5 46 51 PM" src="https://user-images.githubusercontent.com/117620028/205531577-cdb6ee5a-3a81-4aa3-b0aa-1e4afd8a9bd4.png">


## Summary
The Election Committee is able to use this script to analyze these election results, as well as the results from other elections as well.  
In order to analzye results from a different election, a new csv file must be added to the Resources folder nested inside the Election Analysis Folder.  From there, the path must be changed in the script to read this file instead.  In line 15 of [Py_Poll.py](https://github.com/heatherhutchinson211/election_analysis_challenge/blob/main/PyPoll_Challenge_starter_code.py) "election_results.csv" must be updated to the new csv file name.  A new txt file should also be created in the "analysis" folder, and line 19 should be updated to include the path to write to this new file.  From there, running the script should produce similar results based on the different election data provided. 
