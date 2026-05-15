# Data Notes

`labels/` contains manual event-time player-status labels. These are input files, not generated inside the notebook.

`processed/` contains the yearly economics panel, reconciliation outputs, simulation data, and derived summary tables. The notebook can regenerate the derived summary CSVs and charts from the included data.

The original result files do not include occupation, biography, prior live earnings, or satellite-qualifier fields. That is why player status is maintained as a separate label layer.
