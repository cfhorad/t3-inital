# RadCases

An interactive educational platform containing radiologic cases and teaching images for medical students.

## Language

**Case**:
A clinical teaching scenario containing radiologic images, teaching notes, difficulty, and ACR classifications.
_Avoid_: Study, exam, post

**Like**:
A binary flag indicating appreciation for a Case, which also acts as a bookmark to save it to a user's personal list.
_Avoid_: Favorite, upvote

**Quality Rating**:
A 1-to-3 star evaluation of the educational utility of a Case submitted by a user.
_Avoid_: Star rating, score, difficulty

**Test Image**:
A radiologic image in a Case shown to students initially for interpretation or self-testing before the diagnosis is revealed.
_Avoid_: Question image, primary image

**Answer Image**:
A radiologic image in a Case that contains annotations, arrows, or highlights revealing the pathology, shown only after the student requests to reveal the diagnosis.
_Avoid_: Solution image, annotated image

**Diagnosis**:
The final clinical finding or disease identification of a Case, hidden from students initially and revealed alongside Answer Images and Teaching Notes.
_Avoid_: Finding, final answer

**Comment**:
A short textual note written by a user on a Case to discuss clinical findings or ask questions.
_Avoid_: Post, reply, message
