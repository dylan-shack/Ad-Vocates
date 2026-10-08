# D1 Detailed System Design

## 1. Header, Scope, and Conventions

**Project Title:** Ad-Vocates

**Team Members:** Dylan Shackleford, Max Marsh

**Goal:** Create a reliable and efficient tool that improves browsing performance while giving users greater privacy, security, and control over their web experience.

This D1 document provides a more detailed design for the **Filter Request**, **Local Storage**, and **Settings** components from D0. The **Dashboard**, **Statistic Recorder**, **Filter Updater**, and **Filter Validator** components will be detailed further in D2.

### Figure Conventions

The D1 figures use the following conventions:

- Rectangular boxes represent system components or stored data entities.
- Lines between entities represent relationships between stored data.
- Arrows between system components represent the direction of communication or data flow.
- Primary keys are marked with **PK**.
- Foreign keys are marked with **FK**.
- Relationship cardinality is shown using **1:1**, **1:M**, or **M:N** notation.
- Labels on arrows describe the type of data being passed between components.
- Components already defined in D0 keep the same names in D1 so the two designs can be compared directly.
