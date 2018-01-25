# Habits automation system for Mycroft

## Presentation

The habits automation system aims to detect the **user habits** when using [Mycroft](https://mycroft.ai/), and to offer automation of these identified habits.

A habit is defined as a **set of commands** that the user is used to launch together on Mycroft. The habit detection identifies **2 types** of habits:
- **"Time based"** habits (e.g. Everyday at 8 a.m. the user often says "turn on the boiler")
- **"Trigger based"** habits (e.g. Right after saying "Turn on the couch lamps" the user often says "Turn on the kitchen lamps")

Once that a habit has been identified, the user will choose between:
- **automating** the habit: every time the habit "trigger" is detected (time or triggering command), the habit will be reproduced automatically without asking the user
- being given the **option to reproduce the habit**: every time the habit "trigger" is detected, Mycroft will ask the user to ask if he/she wants to reproduce the habit this time
- **not automating** the habit

When it comes to "trigger based" habits, the user is free to chose the command to be used as trigger.

## How does it work?

The habit automation system is composed of **3 Mycroft skills** working together:
1. The [**skill-listener**](https://github.com/PFE1718/mycroft-skill-listener), that logs the user activity. It is also  responsible for launching the 2 other skills when needed.
2. The [**habit-miner**](https://github.com/PFE1718/mycroft-habit-miner-skill), that extracts the habits of the user from the logs.
3. The [**automation-handler**](https://github.com/PFE1718/mycroft-automation-handler), that interacts with the user to offer habits automation. It is also responsible for launching the different commands that are part of the habit automated.

The logs of the user are **stored locally** and never sent to a third party server so you don't have to worry about privacy.

More technically, a habit is composed of a set of **{intent, parameters}**. Intents are defined [here](https://mycroft.ai/documentation/skills/introduction-developing-skills/#skill-terminology) and parameters are the keywords from the [vocab files](https://mycroft.ai/documentation/skills/introduction-developing-skills/#vocab-directory-and-defining-intents). The analysis of the logs is launched automatically when you don't talk to Mycroft for 5 minutes.

You can find more technical details on the repository of each skill.

## How to install the skills?

On a Linux environment with Mycroft installed you can install the skills using
[msm](https://mycroft.ai/documentation/msm/) (Mycroft Skill Manager) by following these steps:
```bash
# get into Mycroft virutalenv
source ~/.virtualenvs/mycroft/bin/activate

# use msm to install the skill
sudo /<PATH_TO_MYCROFT-CORE>/mycroft-core/msm/msm install https://github.com/PFE1718/mycroft-skill-listener
sudo /<PATH_TO_MYCROFT-CORE>/mycroft-core/msm/msm install https://github.com/PFE1718/mycroft-automation-handler
sudo /<PATH_TO_MYCROFT-CORE>/mycroft-core/msm/msm install https://github.com/PFE1718/mycroft-habit-miner-skill
```

Don't forget to leave the virualenv with the command 
```
deactivate
``` 
when you are done!

After successfully installing the 3 skills, the system should start logging 
your activity and eventually offer you to automate habits after they are identified!

**COMMING SOON (hopefully):** direct installation support within Mycroft with:
> Hey Mycroft, install skill listener

## Can you register habits manually?

Yes **you can!**

**COMMING SOON:** A beautiful GUI to manage your habits

For now, if you know a bit about programming, you can dig a bit into the habits storage format which is:
```json
{
    "intents": [ // Array containing the habit's intents
        {
            "parameters": {
                "Application": "atom"
            },
            "name": "-4359987462241748114:LaunchDesktopApplicationIntent",
            "last_utterance": "open atom"
        },
        {
            "parameters": {
                "Application": "firefox"
            },
            "name": "-4359987462241748114:LaunchDesktopApplicationIntent",
            "last_utterance": "open firefox"
        }
    ],
    "trigger_type": "skill", // "skill" for trigger based habits, "time" otherwise
    "automatized": 0, // 0 for no automation, 1 for full automation, 2 for automation offer
    "user_choice": true, // true if you write it yourself
    "triggers": [] // trigger(s) for a trigger base habit
}
```