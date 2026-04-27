# Disk Forensics – Findings

This document summarizes the results of the disk‑level forensic analysis performed on the USB device associated with the deepfake identity‑abuse case study. All findings are based directly on the file system contents and recovered artifacts identified through Sleuth Kit analysis.

---

## 1. Integrity Verification

- A forensic image of the USB device was created using `dd`.
- A SHA‑256 hash were generated for integrity.

---

## 2. File System Overview

Recursive file system enumeration (`fls -r -o 2048 evidence.dd`) revealed a **complete deepfake project environment** stored on the USB device.  
This included source images, scene videos, trained model files, temporary cache frames, and deepfake‑related tools.

### 2.1 Victim Identification

Multiple face images belonging to **Taylor**, the victim in this case study, were present in the `faces/` directory:

- `taylor.jpg`
- `taylor1.jpg`
- `._taylor.jpg`
- `._taylor1.jpg`

The presence of these files confirms that the victim’s likeness was used as input material for deepfake generation.

### 2.2 Deepfake Project Components

Key directories and contents included:

#### **faces/**
Contains face images used for training and swapping:
- Victim images (Taylor)
- Additional individuals: Carly, Carly’s friend, “calc_barb”
- Deleted short‑filename artifact: `_ARLY~16.JPG`

#### **sources/**
Scene videos used as base footage:
- `a_scene.mp4`
- `z_scene.mp4`
- `r_scene.mp4`
- `p_scene.mp4`

#### **model/**
Trained deepfake model components:
- `encoder.bin`
- `decoder.bin`
- `config.json`

#### **cache/**
Temporary frame extractions:
- `tmp_frame_001.jpg`
- `tmp_frame_002.jpg`

#### **tools/**
Deepfake‑related software and scripts:
- `faceswap_installer_v3.exe`
- `mr.deepfake_toolkit.zip`
- `model_converter.py`
- `readme_how_to_train.txt`

#### **guides/**
Instructional material:
- `how_to_make_deepfakes.html`
- `reddit_faceswap_thread.html`
- `guide_face_alignment.pdf`

This structure is consistent with a full deepfake creation workflow.

---

## 3. Deleted Directory: `_utputs`

A deleted directory was identified:

Inode: 273418
Name: _utputs
Status: Not Allocated


This directory contained **four deepfake output videos**, all marked deleted:

- `taylor_swap_01.mp4`
- `carly_swap_01.mp4`
- `carly-friend_swap_01.mp4`
- `calc-barb_swap_01.mp4`

Timestamps show:

- Created: 2026‑04‑21 16:06:00 EDT  
- Written: 2026‑04‑21 16:06:02 EDT  

The extremely short time window between creation and deletion suggests an attempt to conceal the generated deepfake videos.

---

## 4. Recovered Artifacts

Bulk recovery was attempted using:

tsk_recover -o 2048 evidence.dd recovered/


The recovered directory contained:

recovered/
└── finished-vids/
└── faces/
└── _ARLY~16.JPG


The recovered `_ARLY~16.JPG` is a deleted short‑filename version of a face image used in the deepfake process.  
Although only one file was recovered during this run (due to the process being interrupted), the file system listing confirms that **multiple deleted MP4s and images existed**.

---

## 5. Timeline Reconstruction

Based on MAC times and directory structure:

1. Source images (including the victim’s likeness) and scene videos were placed on the USB.
2. Deepfake model files were present, indicating training or inference capability.
3. Deepfake output videos were generated.
4. The `_utputs` directory containing the output videos was deleted within minutes.
5. Only partial remnants (e.g., `_ARLY~16.JPG`) remained recoverable.

This sequence strongly suggests intentional deletion following deepfake creation.

---

## 6. Evidence of Victim Targeting

The presence of multiple images explicitly named after **Taylor**, the victim, demonstrates:

- Direct use of the victim’s likeness  
- Intentional targeting  
- Clear linkage between the recovered deepfake outputs and the victim  

The deleted output video `taylor_swap_01.mp4` further confirms that Taylor was the primary subject of the generated deepfake content.

---

## 7. Offender Assesment

The offender demonstrated **poor operational security**, suggesting inexperience:

- **Use of real victim names in filenames**  
  Advanced offenders avoid embedding identifiable victim information directly into file names.

- **Retention of source images and scene videos**  
  A more sophisticated actor would remove or obfuscate source material.

- **Incomplete deletion of output directories**  
  The `_utputs` directory was deleted but not securely wiped, allowing recovery.

- **Presence of beginner‑oriented tools and guides**  
  The `tools/` and `guides/` directories contained installers, scripts, and tutorials commonly used by novices.

Collectively, these behaviors indicate the individual was **new to deepfake creation and unaware of proper concealment techniques**.

---

## 8. Conclusion

Disk‑level analysis revealed:

- A complete deepfake project environment  
- Multiple deepfake output videos (deleted)  
- Source images, scene videos, model files, and tools  
- A deleted output directory consistent with concealment  
- A recovered face image fragment from the deleted directory  
- Clear evidence that **Taylor was the targeted victim**  
- Strong indicators that the offender was inexperienced  

The evidence overwhelmingly supports that the USB device was actively used to create identity‑abusive deepfake media and that the user attempted to delete or hide the resulting files. Overall, the evidence strongly indicates that the perpetrator was inexperienced and likely young, as a more seasoned offender would not have used the victim’s real name in file and directory names, left source material intact, or relied on beginner‑level deepfake tutorials and guidance from public forums such as 4chan. 
