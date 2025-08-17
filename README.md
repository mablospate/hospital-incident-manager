# Hospital Incident Manager
The intention behind this project is twofold: to put my knowledge of app design, planning and development into practice and to create a portfolio.

As I have no GUI development experience at the start of this project, I plan to create a command-line interface (CLI) front end.
However, I intend to create a graphical user interface for this project in the future, so I will implement a modular architecture to allow for this and other potential changes.

The rest of this document will serve as a record of my work, complete with diagrams and information.

---
# Requirements extraction
In this phase, I used ChatGPT to create an interview from which I could extract the programme requirements based on my intended actions.
## Conversation with the client
- Dr. Ruiz: Hello! Thanks for coming. We’ve been needing something like this for a while. The way we’re currently tracking incidents… well, we’re not, really.
- Me: No problem. I’d be happy to help. Could you tell me what kinds of incidents you need to track?
- Dr. Ruiz: Sure. We have equipment failures — monitors not working, infusion pumps acting up — and also infrastructure issues, like air conditioning failures in operating rooms or water leaks. Then we have IT problems too: system outages, login issues, things like that. And we need to make sure those are addressed quickly.
- Me: Understood. So the idea would be to let hospital staff report these problems as they happen?
- Dr. Ruiz: Exactly. Doctors, nurses, admin staff… anyone should be able to report an incident. We just need a place where they can describe what’s wrong and submit it so it reaches the right team.
- Me: Who would be receiving and handling those reports?
- Dr. Ruiz: Depending on the type of incident, it might go to maintenance, biomedical engineering, or IT. Right now I think just having a shared queue is fine — we’ll sort them internally. Later on we can think about routing by category.
- Me: Makes sense. Would staff need to track the status of their reported incidents?
- Dr. Ruiz: Yes, that’s important. People should be able to see if their issue has been received, if it’s being worked on, if it’s resolved, and so on. It helps avoid confusion and follow-up calls.
- Me: And should the technicians or response teams update the status themselves?
- Dr. Ruiz: Yes. Ideally, whoever is working on an incident can write notes, update the status, and maybe even assign it to someone else on their team if needed.
- Me: How about access levels? Should everyone be able to see everything?
- Dr. Ruiz: No, definitely not. Regular users should only see their own reports. Technicians can see everything in their department. And people like me — managers — should be able to see the full picture across all departments.
- Me: Understood. Would you need reporting features, like seeing stats on the number of incidents or response times?
- Dr. Ruiz: Eventually, yes. For now, just being able to download a list of incidents or filter by date, type, or department would be useful.
- Me: What kind of interface would be easiest for staff to use?
- Dr. Ruiz: I imagine we’ll start small, so a terminal interface or a simple command-line tool is okay for now. But please build it in a way that we can add a proper interface later — maybe web-based or even on tablets — without having to start over.
- Me: Okay. And do you use any existing software for this right now?
- Dr. Ruiz: No, it’s all word of mouth or calls to maintenance. Some departments have spreadsheets. But there’s no central system, and that’s a problem.
- Me: Got it. And just to be clear — would this system handle any patient-related information?
- Dr. Ruiz: No. This is strictly for operational incidents — not clinical events or patient records. Those are handled separately with medical reporting systems.
- Me: That’s good to know. Anything else you’d want to have in the future?
- Dr. Ruiz: Maybe notifications — email or internal alerts when something is urgent or when someone comments on your ticket. But that’s not a must-have yet.
- Me: All right. That’s very helpful. I’ll start from here and design a solution you can grow into.
- Dr. Ruiz: Great. Looking forward to it!

## Requirements extraction
### Functional Requirements
| ID | Description | Priority | Acceptance Criteria |
|---|---|---|---|
| FR1 | As a member of the hospital staff, I want to be able to log any incidents pertaining to equipment, infrastructure or IT so that the right team can take care of them. | High | Any member of the hospital staff can log incidents about equipment malfunction, infrastructure issues and IT problems. |
| FR2 | As a maintenance worker, a biomedical engineer or an IT worker, I want to be able to visualize all the reported incidents in my department so that I can take care of them. | High | All of the reported incidents must be visible to all users categorized as maintenance, biomedical engineering or IT, depending on the department they are assigned by the manager. |
| FR3 | As a member of the hospital staff, I want to be able to track the incident I reported so that I know if it's being worked on, resolved and so on. | Medium | Any member of the hospital staff can access the status of all the incidents they reported at any given time. |
| FR4 | As a user working on an incident, I want to be able to write notes, update its status and assign other users on my team to the incident so that I can sort it out correctly and in an orderly manner. | High | Any user who is assigned to an incident must be able to write notes, update the incident's status and assign other users to it. |
| FR5 | As a manager, I can visualize all the reported incidents and assign them to a department, so that I can sort them correctly. | High | Any manager can visualize all the reported incidents, as well as assign them to a department between maintenance, biomedical engineering and IT. |
| FR6 | As a manager or incident-solver, I want to be able to filter the incidents I have access to by date, type and department, as well as downloading the list so that I can manage them more effectively. | Medium | Any user with the role of manager, maintenance, biomedical engineering or IT can filter their visible incidents by date, type and department, as well as download the list of incidents. |
| FR7 | As a user of the system, I want to be able to use the program through a CLI so that I can access its functionality as early as possible. | High | Any user can access the program's functionality through a CLI. |
| FR8 | As a user of the system, I want to be able to use the program through a GUI so that I can use the program in an easy and intuitive manner. | Low | Any user can access the program's functionality through a GUI. |
| FR9 | As a member of staff, I want to receive an internal notification when someone comments on my reported incident so that I can respond if needed. | Low | When someone comments on an incident, the author will be notified internally. |
| FR10 | As a manager, maintenance worker, biomedical engineer or IT worker, I want to be notified internally when an urgent incident comes up so that it can be solved swiftly. | Low | When an urgent incident is issued, all the managers will get an internal notification, and when this incident is categorized, the members of the target department will be notified as well. |
### Non-Functional Requirements