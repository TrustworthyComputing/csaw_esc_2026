ESC 2026 Challenge Description
==============================

This year's challenge focuses on the protection of a Smart Home Edge Gateway with the help of AI technology. Teams are encouraged to leverage deep learning and Large Language Models (LLMs) to automate these hardware attacks, and exfiltrate sensitive data. Competitors will use the popular **ESP32** platform in a red team/blue team scenario to either launch or mitigate these powerful attacks. They will demonstrate their skills in analyzing concurrent firmware architectures, communication protocols, machine learning elements, and hardware storage mechanisms to identify potential vulnerabilities, perform security analysis, and design robust defenses.

## Challenge structure

The ESC26 competition is divided into two phases:

1. A preliminary **qualification phase**, where teams must compile and submit a short written report. The report should discuss the security implications of concurrent IoT firmware, potential security issues in on-chip neural network classifiers, and security protocols for peripheral storage (such as RFID and EEPROM).

2. A **final phase**, where qualified teams are provided an ESP32-based hardware kit (with a display, MFRC522 RFID reader, and I2C EEPROM) to investigate potential vulnerabilities and attacks to the target platform. Participants will demonstrate their solutions to the provided technical challenges and discuss potential mitigations.

See below for more details on the requirements of each phase.


### Qualification Phase

For this initial phase, teams must submit a **short report** (up to 2 pages, excluding references) exploring the technical themes outlined above. Rather than just summarizing the topics, successful submissions should provide a structured analysis that includes a review of existing techniques and literature related to the themes (context), a clear outline of potential attack methodologies targeting these systems (offense), and an elaborate discussion of how the proposed attacks could be mitigated or prevented (defense).

Submissions will be reviewed by a panel of experts. The evaluation will be based on the **completeness** of the analysis, the **clarity** of the writing, and the overall **technical quality** of the report.

## Final Phase Evaluation and Grading Policies

The final phase will be graded as follows:
- **30% of the final score will be correctness**. The points awarded in this section are based on successfully finding, exploiting, and mitigating the provided challenges and depend on the difficulty of each challenge. The awarded points will be determined systematically by the global organizers and the expert judges.
- **20% of the final score will be AI integration**. This portion of the score is awarded by the panel of expert judges for the creative and effective use of AI/ML/LLM tools to assist or automate the discovery, exploitation, reduction of queries, and/or mitigation of vulnerabilities.
- **20% of the score will be performance and efficiency**. Performance will be evaluated by the panel of expert judges and will encompass the techniques that the participants utilize to address the challenges. Partial solutions will be considered. The metrics include, but are not limited to:
    - Effectiveness of proposed attacks/mitigations
    - Efficiency (number of attempts/queries), repeatability, and creativity
    - Automation of attacks
- **30% of the score will be the quality of the final deliverables (report, judges' presentation, poster)**. The final deliverables will be graded by the judges panel based on organization, clarity of presentation, and detail of explanations.

**Note:
Solutions that involve reverse engineering of the flag from provided binaries, ROP programming, or modification of provided hex files will not be accepted.**


You can refer to the [deliverables page](deliverables.md) for more details on the qualification and final phase deliverables.
<!-- , and the [Final Phase page](Final_Phase.md) for details about how to get started with this year's challenges. -->

To find more information regarding how to register and participate, click [here](logistics.md).
