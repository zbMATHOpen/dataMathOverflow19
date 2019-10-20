# Data Repository MathOverflow - zbMATH links

This repository provides data used for the 
EMS Newsletter article

 __References to academic literature in QA forums - 
  A case study on zbMATH links from MathOverflow__
 _Fabian Müller, Moritz Schubotz Olaf Teschke_
 EMS Newsletter October 19

## Guide to reproduce

* Download MathOverflow dump from 
```
wget https://archive.org/download/stackexchange/mathoverflow.net.7z

```
* Check if the md5 sum of the file is `8011aabf2ae76358abbcf9a493ba9655.`

If the md5 sum does not match you might have downloaded a never version.
To reproduce our results we stored the downloaded zip as [GitHub release]().

<details>
<summary>Take a moment to review the [dataset description](https://archive.org/details/stackexchange) the
meta information</summary>
	
 - Format: 7zipped
 - Files:
   - **badges**.xml
       - UserId, e.g.: "420"
       - Name, e.g.: "Teacher"
       - Date, e.g.: "2008-09-15T08:55:03.923"
   - **comments**.xml
       - Id
       - PostId
       - Score
       - Text, e.g.: "@Stu Thompson: Seems possible to me - why not try it?"
       - CreationDate, e.g.:"2008-09-06T08:07:10.730"
       - UserId
   - **posts**.xml
       - Id
       - PostTypeId
          - 1: Question
          - 2: Answer
       - ParentID (only present if PostTypeId is 2)
       - AcceptedAnswerId (only present if PostTypeId is 1)
       - CreationDate
       - Score
       - ViewCount
       - Body
       - OwnerUserId
       - LastEditorUserId
       - LastEditorDisplayName="Jeff Atwood"
       - LastEditDate="2009-03-05T22:28:34.823"
       - LastActivityDate="2009-03-11T12:51:01.480"
       - CommunityOwnedDate="2009-03-11T12:51:01.480"
       - ClosedDate="2009-03-11T12:51:01.480"
       - Title=
       - Tags=
       - AnswerCount
       - CommentCount
       - FavoriteCount
   - **posthistory**.xml
	   - Id
	   - PostHistoryTypeId
			- 1: Initial Title - The first title a question is asked with.
			- 2: Initial Body - The first raw body text a post is submitted with.
			- 3: Initial Tags - The first tags a question is asked with.
			- 4: Edit Title - A question's title has been changed.
			- 5: Edit Body - A post's body has been changed, the raw text is stored here as markdown.
			- 6: Edit Tags - A question's tags have been changed.
			- 7: Rollback Title - A question's title has reverted to a previous version.
			- 8: Rollback Body - A post's body has reverted to a previous version - the raw text is stored here.
			- 9: Rollback Tags - A question's tags have reverted to a previous version.
			- 10: Post Closed - A post was voted to be closed.
			- 11: Post Reopened - A post was voted to be reopened.
			- 12: Post Deleted - A post was voted to be removed.
			- 13: Post Undeleted - A post was voted to be restored.
			- 14: Post Locked - A post was locked by a moderator.
			- 15: Post Unlocked - A post was unlocked by a moderator.
			- 16: Community Owned - A post has become community owned.
			- 17: Post Migrated - A post was migrated.
			- 18: Question Merged - A question has had another, deleted question merged into itself.
			- 19: Question Protected - A question was protected by a moderator
			- 20: Question Unprotected - A question was unprotected by a moderator
			- 21: Post Disassociated - An admin removes the OwnerUserId from a post.
			- 22: Question Unmerged - A previously merged question has had its answers and votes restored.
		- PostId
		- RevisionGUID: At times more than one type of history record can be recorded by a single action.  All of these will be grouped using the same RevisionGUID
		- CreationDate: "2009-03-05T22:28:34.823"
		- UserId
		- UserDisplayName: populated if a user has been removed and no longer referenced by user Id
		- Comment: This field will contain the comment made by the user who edited a post
		- Text: A raw version of the new value for a given revision
			- If PostHistoryTypeId = 10, 11, 12, 13, 14, or 15  this column will contain a JSON encoded string with all users who have voted for the PostHistoryTypeId
			- If PostHistoryTypeId = 17 this column will contain migration details of either "from <url>" or "to <url>"
		- CloseReasonId
			- 1: Exact Duplicate - This question covers exactly the same ground as earlier questions on this topic; its answers may be merged with another identical question.
			- 2: off-topic
			- 3: subjective
			- 4: not a real question
			- 7: too localized
   - **postlinks**.xml
     - Id
     - CreationDate
     - PostId
     - RelatedPostId
     - PostLinkTypeId
       - 1: Linked
       - 3: Duplicate
   - **users**.xml
     - Id
     - Reputation
     - CreationDate
     - DisplayName
     - EmailHash
     - LastAccessDate
     - WebsiteUrl
     - Location
     - Age
     - AboutMe
     - Views
     - UpVotes
     - DownVotes
   - **votes**.xml
     - Id
     - PostId
     - VoteTypeId
        - ` 1`: AcceptedByOriginator
        - ` 2`: UpMod
        - ` 3`: DownMod
        - ` 4`: Offensive
        - ` 5`: Favorite - if VoteTypeId = 5 UserId will be populated
        - ` 6`: Close
        - ` 7`: Reopen
        - ` 8`: BountyStart
        - ` 9`: BountyClose
        - `10`: Deletion
        - `11`: Undeletion
        - `12`: Spam
        - `13`: InformModerator
     - CreationDate
     - UserId (only for VoteTypeId 5)
     - BountyAmount (only for VoteTypeId 9)
</details>

<details>
<summary>extract the file</summary>

```bash
physikerwelt@math-docker:~/mathoverflow$ 7z e mathoverflow.net.7z

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel Core Processor (Skylake, IBRS) (506E3),ASM,AES-NI)

Scanning the drive for archives:
1 file, 317278828 bytes (303 MiB)

Extracting archive: mathoverflow.net.7z
--
Path = mathoverflow.net.7z
Type = 7z
Physical Size = 317278828
Headers Size = 349
Method = BZip2
Solid = +
Blocks = 7

Everything is Ok

Files: 8
Size:       1725254003
Compressed: 317278828
```
<details>
<summary>extract posts with references to zbmath.org</summary>

```bash
physikerwelt@math-docker:~/mathoverflow$ wc -l Posts.xml
252154 Posts.xml
physikerwelt@math-docker:~/mathoverflow$ grep 'zbmath.org' Posts.xml > zblPosts.xml
physikerwelt@math-docker:~/mathoverflow$ wc -l zblPosts.xml
774 zblPosts.xml
```

</details>

In the following we analyse
[Posts](https://github.com/ag-gipp/19emsMathOverflow/releases/download/v0.1/PostHistory.7z)
that contain the string `zbmath.org`.

## Files in the repository

The following csv files, were all created using search
and replace in a standard text editor.


* [counts.csv](counts.csv) contains the number of all posts
(not only with links to zbMATH) grouped by month.
The first incomplete year and the current year were deleted.
* [mathoverflow-links-stat.xlsx](mathoverflow-links-stat.xlsx) is an Microsoft Excel File that analyses the date
distribution of the MathOverflow posts with links to zbMath in the table
dates.