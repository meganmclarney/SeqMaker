# SeqMaker Portable - Complete Guide

## 📖 Table of Contents
- [What is SeqMaker?](#what-is-seqmaker)
- [Quick Start](#quick-start)
- [Setup Instructions](#setup-instructions)
- [How to Use](#how-to-use)
- [Troubleshooting](#troubleshooting)
- [Customizing Copyright Info](#customizing-copyright-info)
- [Advanced Tips](#advanced-tips)

---

## What is SeqMaker?

SeqMaker automatically converts video files (.mov, .mp4) into JPEG image sequences at multiple frame rates (1fps, 2fps, 3fps, 4fps, 5fps). It's portable - works from anywhere: Desktop, Documents, external drives, wherever you need it!

**Features:**
- ✅ Mathematically accurate frame extraction
- ✅ Maximum quality JPEGs
- ✅ Automatic copyright metadata embedding
- ✅ Batch processing (handles multiple videos)
- ✅ Easy start/stop control
- ✅ Works on internal and external drives

---

## Quick Start

### First Time Setup (5 minutes)

1. **Create your SeqMaker folder** anywhere you want:
   - Desktop: `~/Desktop/SeqMaker/`
   - Documents: `~/Documents/SeqMaker/`
   - External drive: `/Volumes/YourDrive/SeqMaker/`

2. **Copy these files into your SeqMaker folder:**
   - `SeqMaker-Portable.sh`
   - `START-SeqMaker.command`
   - `STOP-SeqMaker.command`

3. **Create fps folders** (only create the ones you need):
   ```bash
   cd ~/Desktop/SeqMaker
   mkdir 1fps 2fps 3fps 4fps 5fps
   ```

4. **Make files executable** (one-time only):
   ```bash
   cd ~/Desktop/SeqMaker
   chmod +x SeqMaker-Portable.sh START-SeqMaker.command STOP-SeqMaker.command
   ```

5. **Remove quarantine** (for downloaded files):
   ```bash
   xattr -dr com.apple.quarantine START-SeqMaker.command STOP-SeqMaker.command
   ```

Done! You're ready to process videos.

---

## Setup Instructions

### Your Folder Structure Should Look Like:

```
SeqMaker/
├── START-SeqMaker.command       ← Double-click to start
├── STOP-SeqMaker.command        ← Double-click to stop
├── SeqMaker-Portable.sh         ← Main script (don't touch)
│
├── 1fps/                        ← Empty marker folders
├── 2fps/                        ← (create only the rates you want)
├── 3fps/
├── 4fps/
├── 5fps/
│
├── myvideo.mov                  ← Your videos go here
├── anothervideo.mov
│
├── done/                        ← Auto-created: processed videos
│   └── myvideo.mov
│
├── myvideo/                     ← Auto-created: extracted frames
│   ├── 1fps_189/
│   │   ├── frame_000001.jpg
│   │   ├── frame_000002.jpg
│   │   └── ...
│   ├── 2fps_378/
│   ├── 3fps_568/
│   ├── 4fps_757/
│   └── 5fps_947/
│
└── anothervideo/
    ├── 1fps_270/
    └── ...
```

### Important Notes:

- **fps folders are markers**: They stay empty - just tell SeqMaker which rates to extract
- **Only create fps folders you need**: Want only 1fps and 3fps? Just create those two
- **done/ folder auto-creates**: SeqMaker makes this when it moves processed videos
- **Video folders auto-create**: Named after each video file

---

## How to Use

### Perfect Workflow:

#### Step 1: Prepare Your Videos
1. Copy ALL your videos into the SeqMaker folder
2. Wait for copying to finish completely
3. Don't start SeqMaker until copying is done!

#### Step 2: Start Processing
1. **Double-click** `START-SeqMaker.command`
2. Terminal window opens
3. You'll see: "SeqMaker is now watching..."
4. Processing begins automatically

#### Step 3: Monitor Progress
Watch the Terminal window for:
```
Processing: myvideo.mov
Video duration: 189s
Converting at 1fps...
✓ 1fps: 189 frames extracted
Converting at 2fps...
✓ 2fps: 378 frames extracted
...
✓ Processing complete for myvideo
```

#### Step 4: Stop When Done
1. **Double-click** `STOP-SeqMaker.command`
2. Or press **Ctrl+C** in the Terminal window
3. Wait for current video to finish (it won't interrupt mid-process)

#### Step 5: Get Your Files
- **Processed videos**: Check `done/` folder
- **Extracted frames**: Check folders named after each video

### Processing Multiple Videos

SeqMaker automatically processes all videos in the folder:
- Drop in 5, 10, 50 videos - doesn't matter!
- They process one at a time, in order
- Each video goes through all fps rates you've set up
- Can take hours for many videos - that's normal!

### Adding Videos While Running

You can add more videos while SeqMaker is running:
1. Just drop them in the folder
2. SeqMaker will detect and process them
3. No need to stop and restart

---

## Troubleshooting

### "File could not be executed" or "Not have appropriate access privileges"

**Solution:**
```bash
cd ~/Desktop/SeqMaker
chmod +x START-SeqMaker.command STOP-SeqMaker.command SeqMaker-Portable.sh
```

### "Apple could not verify... free of malware"

This happens with downloaded files.

**Solution 1 - Right-click method:**
1. Right-click `START-SeqMaker.command`
2. Choose "Open"
3. Click "Open" again in the dialog
4. Repeat for STOP-SeqMaker.command

**Solution 2 - Terminal method (faster):**
```bash
cd ~/Desktop/SeqMaker
xattr -dr com.apple.quarantine START-SeqMaker.command STOP-SeqMaker.command
```

### START doesn't open Terminal window

**Solution 1:**
Right-click → Open With → Terminal

**Solution 2:**
Check file extension is `.command` not `.command.txt`:
```bash
cd ~/Desktop/SeqMaker
mv START-SeqMaker.command.txt START-SeqMaker.command
mv STOP-SeqMaker.command.txt STOP-SeqMaker.command
```

### "[q] command received" - Processing stops early

This happens if you start SeqMaker BEFORE videos finish copying.

**Solution:**
1. Click STOP-SeqMaker
2. Delete incomplete folders
3. Move videos back from `done/` if needed
4. **Wait for ALL videos to finish copying**
5. Click START-SeqMaker again

**Prevention:**
Always copy videos FIRST, then start SeqMaker!

### "exiftool: command not found"

Copyright metadata requires exiftool.

**Solution:**
```bash
brew install exiftool
```

Or download from: https://exiftool.org

### Frame counts seem wrong

Check the video duration:
```bash
ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 myvideo.mov
```

Expected frames = duration × fps (rounded)
- 189 second video at 2fps = 378 frames
- 270 second video at 3fps = 810 frames

Counts should be within ±1-2 frames due to video encoding.

### Videos not processing

**Check:**
1. Are fps folders created? (`ls -la` to see folders)
2. Are videos actually .mov or .mp4? (not .avi, .mkv, etc.)
3. Is SeqMaker running? (Terminal window open?)
4. Are videos in the root folder, not subfolders?

### SeqMaker won't stop

**Force stop:**
```bash
# Find the process
ps aux | grep SeqMaker

# Kill it (replace XXXXX with actual PID)
kill XXXXX
```

Or just close the Terminal window.

### External drive disconnected mid-process

If your external drive disconnects:
1. Reconnect the drive
2. Click STOP-SeqMaker
3. Check what's in `done/` - those are finished
4. Delete any incomplete folders
5. Start again with remaining videos

### Running out of disk space

Image sequences take up a LOT of space!

**Estimate:**
- 1 second of video = 1 frame at 1fps ≈ 200KB
- 1 second of video = 5 frames at 5fps ≈ 1MB
- 1 minute video at all rates (1-5fps) ≈ 150MB
- 10 minute video at all rates ≈ 1.5GB

**Solutions:**
- Only create fps folders you actually need
- Process on external drives with more space
- Delete frame sequences after you're done with them

### Videos in wrong orientation

SeqMaker preserves the original video orientation. If frames appear rotated:
- The video was shot rotated
- Check the original video orientation
- Frames are correct - the video itself is rotated

---

## Customizing Copyright Info

Every extracted JPEG frame includes embedded copyright metadata. By default:
```
Artist: Megan McLarney
Copyright: Copyright © 2026 Megan McLarney. All Rights Reserved.
```

### How to Change Copyright Information

#### Method 1: Edit the Script (Easy!)

1. **Open the script:**
   - Right-click `SeqMaker-Portable.sh`
   - Choose "Open With" → "TextEdit"

2. **Find this section** (around line 110-117):
   ```bash
   # Add copyright metadata to all frames
   echo "Adding copyright metadata..."
   local year=$(date +%Y)
   exiftool -overwrite_original \
       -Artist="Megan McLarney" \
       -Copyright="Copyright © $year Megan McLarney. All Rights Reserved." \
       -CopyrightNotice="Copyright © $year Megan McLarney. All Rights Reserved." \
       -Rights="Copyright © $year Megan McLarney. All Rights Reserved." \
       -Creator="Megan McLarney" \
       "$temp_dir"/*.jpg 2>/dev/null
   ```

3. **Change the name:**
   - Replace "Megan McLarney" with your name, studio name, company, etc.
   - Example: "John Smith Studios"
   - Example: "Creative Company LLC"

4. **Save the file**

5. **Done!** Next time you process videos, they'll have the new copyright info.

#### Method 2: Find & Replace (Faster!)

In TextEdit:
1. Press **Command+F** (Find)
2. Type: `Megan McLarney`
3. Click "Replace All"
4. Type your new name
5. Click "Replace All"
6. Save

#### What You Can Customize:

**Change the artist name:**
```bash
-Artist="Your Name Here"
```

**Use a specific year instead of current year:**
```bash
-Copyright="Copyright © 2025 Your Name. All Rights Reserved."
```

**Change the rights statement:**
```bash
-Copyright="Copyright © 2026 Your Name. Creative Commons BY-NC."
```

**Remove copyright entirely:**
Delete the entire exiftool section (lines with exiftool and all the -Artist, -Copyright lines)

### Updating Copyright on Existing Frames

Already extracted frames and want to update the copyright?

```bash
cd ~/Desktop/SeqMaker/videoname/1fps_189

exiftool -overwrite_original \
    -Artist="New Name Here" \
    -Copyright="Copyright © 2026 New Name Here. All Rights Reserved." \
    -CopyrightNotice="Copyright © 2026 New Name Here. All Rights Reserved." \
    -Rights="Copyright © 2026 New Name Here. All Rights Reserved." \
    -Creator="New Name Here" \
    *.jpg
```

This updates all JPEGs in that folder.

### Verify Copyright Was Added

Check a frame:
```bash
exiftool frame_000001.jpg | grep Copyright
```

Or in Finder:
1. Right-click any frame
2. Get Info
3. Look under "More Info"

---

## Advanced Tips

### Process Only Specific Frame Rates

Only want 1fps and 3fps? Just create those folders:
```bash
mkdir 1fps 3fps
```

Don't create 2fps, 4fps, 5fps - SeqMaker will skip them.

### Use Different Locations

Copy your entire SeqMaker folder anywhere:
```bash
# Desktop
cp -r ~/Desktop/SeqMaker ~/Documents/Project1/SeqMaker

# External drive
cp -r ~/Desktop/SeqMaker /Volumes/Bunny04/ProjectA/SeqMaker
```

Each location works independently!

### Keyboard Shortcuts

- **Start SeqMaker**: Just double-click START
- **Stop SeqMaker**: Ctrl+C in Terminal (or double-click STOP)
- **Force quit Terminal**: Command+Q

### View Processing Speed

Watch the Terminal output:
```
frame= 189 fps= 20 q=2.0 Lsize=N/A time=00:03:09 bitrate=N/A speed=19.5x
```

`speed=19.5x` means processing 19.5× faster than real-time!

### Check Disk Space

Before processing:
```bash
df -h ~/Desktop/SeqMaker
```

### Move Completed Work

After processing, move everything:
```bash
# Move processed videos somewhere safe
mv ~/Desktop/SeqMaker/done/*.mov ~/Movies/Processed/

# Move frame sequences to project folder
mv ~/Desktop/SeqMaker/videoname ~/Documents/Project/Frames/
```

### Clean Up Between Batches

```bash
cd ~/Desktop/SeqMaker

# Remove all processed videos
rm -rf done/*

# Remove all frame folders
rm -rf */

# Keep: START, STOP, SeqMaker-Portable.sh, fps folders
# This leaves you ready for the next batch
```

### Check What's Running

```bash
# See if SeqMaker is running
ps aux | grep SeqMaker

# See if ffmpeg is running
ps aux | grep ffmpeg
```

### Monitor CPU Usage

Open Activity Monitor:
1. Applications → Utilities → Activity Monitor
2. Look for "ffmpeg" process
3. Should be using 200-400% CPU (2-4 cores)

---

## Requirements

### Required:
- macOS (tested on macOS 11+)
- `ffmpeg` (install with: `brew install ffmpeg`)
- `fswatch` (usually pre-installed)
- Enough disk space (image sequences are large!)

### Optional:
- `exiftool` for copyright metadata (install with: `brew install exiftool`)

### Installing Homebrew (if needed):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then install requirements:
```bash
brew install ffmpeg exiftool
```

---

## File Types Supported

### Input (Videos):
- `.mov`, `.MOV`
- `.mp4`, `.MP4`

### Output (Frames):
- `.jpg` (maximum quality, q=2)

---

## FAQ

**Q: Can I process videos while SeqMaker is running?**  
A: Yes! Just drop them in the folder. SeqMaker detects and processes them.

**Q: How long does processing take?**  
A: Roughly 15-20× real-time speed. A 10-minute video takes ~30 seconds per fps rate.

**Q: Can I cancel mid-processing?**  
A: Yes, click STOP. Current video will finish, then stop.

**Q: What if I only want one fps rate?**  
A: Only create that one fps folder. Delete or don't create the others.

**Q: Do I need all those fps folders?**  
A: No! Only create the ones you actually want.

**Q: Can I change the output quality?**  
A: Yes, edit the script and change `-q:v 2` to another value (2 is highest quality).

**Q: Where do processed videos go?**  
A: Automatically moved to `done/` folder after processing.

**Q: Can I process the same video again?**  
A: Yes, move it back from `done/` folder and it'll process again.

**Q: What's the `.seqmaker.pid` file?**  
A: Internal file - tracks if SeqMaker is running. Don't delete while running.

**Q: Can I run multiple SeqMakers at once?**  
A: Yes, in different folders on different drives. Not in the same folder!

---

## Support

**Check the logs:**
```bash
cd ~/Desktop/SeqMaker
cat .seqmaker.pid  # See if running
```

**Common error messages:**

- "command not found" → Install the missing tool (ffmpeg, exiftool, fswatch)
- "Permission denied" → Run `chmod +x` on the files
- "No such file or directory" → Check your paths

**Still stuck?**
Double-check:
1. Files are executable (`ls -la` should show `-rwxr-xr-x`)
2. fps folders exist
3. Videos are actually .mov or .mp4
4. You're in the right folder

---

## Credits

SeqMaker Portable - Frame-perfect video to image sequence converter  
Version 2.0

Includes automatic copyright metadata embedding with proper legal formatting.

---

**Happy frame extracting!** 🎬✨
