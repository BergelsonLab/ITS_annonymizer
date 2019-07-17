# ITS file anonymizer

DISCLAIMER: Please read this readme carefully so that you ensure you understand what data are being anonymized, and what data are being left in their original format.
Researchers may also wish to rename their ITS filenames before public release, as filenames contain potentially identifying information (such as child ID #), which this program does not alter.

The anonymizer processes files line-by-line via a text editor, preserveing the original structure of the ITS file.
It allows the user to choose the folder that contains the original ITS files, and to choose the folder where they would like to save their anonymized files.

## To use

1. Double click on the file 'its_anon_gui2.py' This will launch a user interface with several buttons
2. Select input folder (allows user to choose input ITS files)
3. Select output folder (allows user to choose where to save anonymized files - it is recommended to have an empty output folder already made!)
4. Click "Fully anonymize files" (anonymizes all the sensitive/non-anonymous data in the files - see below for details)

#In development:

5. Partially anonymize files (anonymizes only data specified)
6. checkbuttons: when selected, the data in that row of the ITS file will be left in ITS original format

## Anonymization and Rationale

Several different generic strings are used to replace identifying information, stored in `replacement_dict.json`. This file can be easily modified to include other information that needs to be private.

Several data points are anonymized when this program is run. See below for a description of and rationale for replacing each item.

1. Child's birthdate:
	dob replaced with 1000-01-01
	NOTE: Child's chronological and estimated developmental ages are NOT anonymized, as this information may be needed for meaningful data analyses.
2. Filename:
	filename replaced with new_filename_1001
	In the ITS file, the filename is listed, which contains the recording upload date. Knowing the upload date and child's chronological age could allow for birthdates to be calculated.
	NOTE: filename here is NOT the same as the name the file is saved as on your disk. This program makes no changes to the names of files, it only alters information within files.
3. Date information:
	file upload date, transfer time, recording date, enrollment date, etc. replaced with 1000-01-01
	Several places throughout the ITS, information about dates that correspond closely to the date the recording was made can be found. Rationale for anonymizing this information is the same as for the filename.
4. Child ID:
	id replaced with A999
	Child ID is the ID given to the child by the lena system, and could be linked back to a participant name, depending on each lab's internal data storage setup.
5. Timezone:
	time zone replaced with AAA
	The name of the recording's timezone is anonymized. However, the short version of the timezone name and whether or not they use daylight savings time is NOT anonymized, as this information may be needed for data analyses.
6. Log file name:
	logfile replaced with exec10001010T100010Z_job00000001-10001010_101010_100100.upl.log
	The logfile name contains information about upload date, and the Child ID
	
There are some items that we decided to keep un-anonymous:

1. Time information:
	Specific time (hour:min:sec) information is necessary to keep to allow for time-of-day analyses.
2. Child key:
	The child key is a Lena-generated number that is specific to each individual recording, but can't be linked to other personal information about the participant.
3. Gender:
	Knowing the gender of the child without birthdate information does not reveal much about the participant, but could be useful in some analyses.
4. Recording device serial ID and version:
	Information about the recording device and software it is using are kept. In the unlikely event that a given set of recorders or software are faulty, data that were collected on those devices can be flagged and excluded if necessary.
5. Chronological age:
	This information is left as is to allow for age-effect analyses.
	Since date of birth and recording date information are being anonymized, chronological age information does not allow any private information about the participant to be calculated.
6. Group ID:
	Recordings can be collected as a part of a larger group, and the group ID helps let researches know more information about what group or population the recording belongs to.
	However, in some labs the group ID may be the SAME as the child ID. In these cases, changes will have to be made to the program to ensure this gets anonymized.
	
	
## Checkbuttons (in development)

Each checkbutton, if checked will exclude the corresponding row of data from anonymization. Data in each row are:

1. PrimaryChild row: date of birth, gender, child key
2. ITS row: file name, time file was created
3. ChildInfo row: date of birht, gender
4. SRDInfo row: serial number
5. Child row: date of birth, gender, enrollment date, child ID, child key
6. Time data rows: all instances of timestamps throughout the file (if "only_time" is set to true, the timestamps, but not dates, will be preserved)

## Contact

If you have questions about how to use the anonymizer, bug fixes, or improvements, please contact Sarah at smacewan@mts.net