## Experience with Copilot and Git-Based BI Development

Working on the BrewMetrics project showed me that AI assistance is most useful when it is treated as a development aid rather than as a replacement for checking the result. Copilot was able to provide useful starting points for DAX calculations, particularly for time-based calculations, cumulative totals and RANKX-based city comparisons. Simple measures required very little modification, while calculations involving date context needed more attention before they produced the expected results.

One important part of the process was validating the suggested DAX against the actual Power BI model. The names of the fact and dimension columns had to match the project structure, and the measures had to respond correctly when filters or slicers were applied. This made it clear that a generated formula can look correct syntactically but still require changes to work properly with a particular data model.

Using Git also changed the development process compared with a normal Power BI lab. Instead of treating the PBIX/report as one final file, the project was developed through identifiable stages. The schema, measures, report and documentation could be tracked separately. This made the development process easier to review and provided a record of how the project evolved.

Overall, combining Power BI, Copilot and Git made the work more structured and encouraged me to verify each change rather than accepting generated output without testing it.