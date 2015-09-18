##### Files

- How does competitive price work?
  - Average of existing hosts?
  - Does this price differ widly between peers?
  - duration
  - Per MB / GB / TB
- Is the term Contract and File the same? (Files = Contracts)
- Any way to get a cost preview before uploading a files or group of files?
- what is stored locally after a file is uploaded?
  - .sia?
  - ASCII?
  - if you lose this key, how do you get files back?
  - What if computer crashes, do you lose access to all your files even if you backed up your wallet?
- When a file is uploaded when is the ASCII available and where is it stored?
- If you delete a file will shared access be removed as well?
- What exactly happens when you use ASCII to view a file?
   - Is an entry created in your file list a reference.
   - Does this give you access to delete their file or only your copy/reference?
- How many hosts will be tracked per client?
- Can each file/contract be associated with its own sia or bitcoin address? 
  - Potentailly a lot of private keys to maintain. Is this any different than it is now?
- Can one user give access to all thier files to another user?
- Define uSC, mSC? What is returned in current API?
- What is the unlockHash in hostDB API call, should it be public?
- What happens when you click Announce? Under what conditions would someone have to announce again?


##### General

- What public/nonpublic metadata is capable of being stored on SIA’s blockchain?
- How do you retrieve network wide contract count?

Proposed API/siad Features
=====

Contracts/Renter
- Ability to stop accepting new contracts (on/off switch)
- Easier key to identify and access a file. 
- Define config on a per file/contract basis
- Submit file(s) and start upload in separate API command.
- API call to check if you are announced to the network or not.

Wallet
- 
