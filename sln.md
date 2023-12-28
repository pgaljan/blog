# Software Lifecycle Navigator

Interoperability and coexistence is a critical topic for anyone who deal with complex systems  System Architects and planners need to fully understand the lifecycle of every component in their system.  Developers and system testers need to have a clear understanding of the versions of software and hardware they are likely to encounter in the field so they can develop to realistic use cases and test likely scenarios.  Operations and support personnel need to have a very clear view of what is supported and maintained at any given time to take correct action.  

[Endoflife.date](https://endoflife.date/) is a project that tracks release metadata for open source and proprietary software and hardware projects.  [Software Lifecycle Navigator](https://app.powerbi.com/view?r=eyJrIjoiYzRmOTY1MDYtZmE1ZC00MzA5LWFhMjYtMTIzM2Q0MWMwYjBlIiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9) leverages the endoflife.date API to periodically retrieve those data elements required to build a common GANTT chart for a subset of projects available on Endoflife.date.

With the resulting dashboard, users can quickly determine temporal overlap between projects, determine which are currently maintained, explore past release practices for that project, and get a solid idea of the project's release and support cadence.

[![dashboard][1]][2]

[1]:  https://github.com/pgaljan/blog/assets/11296072/034e0696-be08-4541-bf90-3aae0b00e6ec
[2]:  https://app.powerbi.com/view?r=eyJrIjoiYzgyYThhYzAtNDY1Ny00MWYyLWJhZmEtNjE5OGRiZWNiY2Y2IiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9 "Try out the live dashboard"
##### [Live demo](https://app.powerbi.com/view?r=eyJrIjoiYzRmOTY1MDYtZmE1ZC00MzA5LWFhMjYtMTIzM2Q0MWMwYjBlIiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9)

The dashboard updates automatically on a weekly basis.  Please reach out if you'd like to see a project added to the dashboard.