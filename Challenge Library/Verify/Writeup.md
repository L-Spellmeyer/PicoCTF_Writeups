The author would like readers to recognizes that this document was formatted and edited with the assistance of the AI tool ChatGPT, utilizing information and insights provided by the author.

# picoCTF Challenge: File Integrity Verification

## Summary

**Challenge Description:**  
*“People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.”*

**Classification:** Easy  
**Keywords:** Forensics, checksum, SHA-256, `grep`, `sha256sum`, picoCTF 2024  
**Hints Provided:**
- Checksums can validate a file's completeness and originality. A mismatch in checksums indicates a different file.
- The SHA-256 checksum for files in a directory can be created with `sha256sum <directory>/*`.
- Command outputs can be piped to refine searches, such as using `|` to direct `sha256sum` results to `grep` for efficient searching.

---

## Step-by-Step Solution Guide

1. **Review Each File’s Content**  
   - **Command:** `cat <file>`
   - **Explanation:** `cat` displays the contents of a specified file. Starting by reading each file allows us to understand what is available, particularly focusing on `checksum.txt` which holds the valid SHA-256 hash for the genuine flag. This hash will be essential for identifying the correct file.

2. **Generate SHA-256 Hashes for All Files in the Directory**  
   - **Command:** `sha256sum files/*`
   - **Explanation:** `sha256sum` generates a unique SHA-256 hash for each file, helping us verify each file’s integrity. Running this on `files/*` produces a list of hashes for every file in the “files” directory. We’ll compare these hashes against the known hash in `checksum.txt`.

3. **Search for the Matching Hash Using `grep`**  
   - **Command:** `sha256sum files/* | grep -F "$(cat checksum.txt)"`
   - **Explanation:** Piping (`|`) the output of `sha256sum files/*` to `grep` lets us quickly search for the hash that matches the one in `checksum.txt`. The command `$(cat checksum.txt)` inserts the contents of `checksum.txt` as the search term, while `-F` treats it as a fixed string to avoid interpretation errors. If a match is found, `grep` returns the name of the correct file.

4. **Decrypt the Verified File Using `decrypt.sh`**  
   - **Command:** `./decrypt.sh <file>`
   - **Explanation:** This step runs the provided decryption script on the identified file (replace `<file>` with the matching file name). If you receive a “permission denied” error, run `chmod +x decrypt.sh` to make the script executable. Once decrypted, the script outputs the flag.

5. **Retrieve the Flag**  
   - **Flag Returned:** `picoCTF{trust_but_verify_451fd69b}`
   - **Explanation:** Successfully decrypting the file confirms its authenticity and completes the challenge, revealing the flag.

---

## Key Takeaways

1. **Importance of Checksums for File Integrity**  
   This challenge underscored the utility of checksums like SHA-256 in confirming file authenticity. Matching a file’s checksum against a known valid checksum ensures the file’s origin and integrity, which is especially valuable in security contexts.

2. **Effective Use of Piping and `grep`**  
   Using piping to send `sha256sum` output directly to `grep` demonstrated a powerful technique for streamlining searches. This approach simplifies workflows when working with multiple files, allowing direct search and comparison against specific data without intermediate files.

3. **Troubleshooting and Refining Commands**  
   The challenge illustrated the importance of refining commands to achieve the desired results. Initial attempts that did not yield results required small adjustments, such as searching specific data or adjusting the command syntax, which ultimately led to success.

4. **Command Alternatives and Flexibility**  
   While `cat` was effective for viewing file contents, exploring alternatives like `head` and `tail` could be useful for viewing larger files. The experience reinforced the flexibility needed in using Linux command-line tools, as adapting approaches can expedite solutions in file verification and integrity scenarios.

---
