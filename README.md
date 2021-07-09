# Election_Analysis With Python

## Overview of Project

### Purpose
The purpose of this project was to submit the updated election audit results to the election commission. The election audit was already submitted to election commision for review and the election commission requested some additional data to complete the audit.

Additional data to complete the audit:
* The voter turnout for each county
* The percentage of votes from each county out of total count
* The county with highest turnout

This project uses Visual Studio Code and Python to create the technical analysis and writing skills to write a written report based on analysis.

This project consits of two technical analysis deliverables and a written report to deliver the results. 

* Deliverable 1: The Election Results Printed to the Command Line
* Deliverable 2: The Election Results Saved to a Text File
* Deliverable 3: A written Analysis of the Election Audit (README.md)

### Election-Audit Results

The Following Image shows the Election Results printed on the Command Line

![Result on Terminal](https://user-images.githubusercontent.com/25447945/125129752-8afdcc80-e0c5-11eb-922a-a7f288ea1a97.png)

The Following Image shows the Election Results saved to a text file

![Result on txt](https://user-images.githubusercontent.com/25447945/125129924-bda7c500-e0c5-11eb-9e64-884f8b332d48.png)

Based on the Results shown above we can answer the following outcomes

1. How many votes were cast in the congressional election?
   - There were total of 369,711 votes cast in the congressional election.
   
2. Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.
   * Number of Votes and Percentage of total votes for each county:
     - Jefferson: 10.5% (38,855)
     - Denver: 82.8% (306,055)
     - Arapahoe: 6.7% (24,801)
3. Which county had the largest number of votes?
   * Denver had the largest votes with 306,055 votes
   
4. Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
   * Number of Votes and Percentage of total votes for each candidates:
     - Charles Casper Stockham: 23.0% (85,213)
     - Diane DeGette: 73.8% (272,892)
     - Raymon Anthony Doane: 3.1% (11,606)
5. Which candidate won the election, what was their vote count, and what was their percentage of the total votes?
   * The Candidate that won the election is Diane DeGette with 272,892 votes and 73.8% of the total votes.

### Election-Audit Summary

The Following script was used to show the results of the election.

```
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join( "Resources", "election_results.csv")
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
county_largest_votes = 0

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

            # Add the county name to the county list.
            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1


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
    for county_name in county_votes:
        # 6b: Retrieve the county vote count.
         c_votes = county_votes[county_name]
         
         
        # 6c: Calculate the percentage of votes for the county.
         vote_county_percentage = float(c_votes) / float(total_votes) * 100
         county_results = (f"{county_name}: {vote_county_percentage:.1f}% ({c_votes:,})\n")
         # 6d: Print the county results to the terminal.
         print(county_results, end="")

         # 6e: Save the county votes to a text file.
         txt_file.write(county_results)
         # 6f: Write an if statement to determine the winning county and get its vote count.
         if (c_votes > county_largest_votes):
            county_largest_votes = c_votes
            largest_county = county_name
            
    # 7: Print the county with the largest turnout to the terminal.
    
    winning_county_summary = (
        f"\n"
        f"-------------------------\n"
        f"Largest County Turnout: {largest_county}\n"
        f"-------------------------\n")
    
    print(winning_county_summary)

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)

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
With the help of this script we were able to easlily tallly the results and could declare the candidate who won the elections. This script would be very helpful for the upcoming elections and we could declare the winner in no time.

The script can be modified to find the most number of votes for different filters other than candidates and county, we can filter using the ethnicity or race of each candidate. This script can also be modified to determine patterns among the characteriscts. We could test the percentage of voters by county against each candidate. This could show us that which candidate is popular within a geographical area. 

This script would be very benefital to the election commissions and would make the life easy for them. This will also help the candidates relieve their stress level as their stress level would be higher while waiting for their results. 
