# Report templates

`relatorio_bimestral_template.docx` is a docxtpl (Jinja2-in-docx) template,
generated from the partner's real mockup by
`../../scripts/build_relatorio_bimestral_template.py` — run that script
again (pointed at a revised mockup) rather than hand-editing this file.

The tag style matches `ecoscope-workflows-ext-apn`'s real, working
`generate_patrol_report` template: `{{ var }}` for scalars/images,
`{%tr for item in list %}` / `{%tr endfor %}` for table row-loops. Context
keys are built by `ecoscope_workflows_ext_icmbio.tasks.
prepare_bimonthly_report_context` (`period`, `total_patrols`,
`total_distance_km`, `total_participants`, `patrols`, `events_summary`,
`camera_traps`, `threats`, `roadkill`, `grid_summary`) plus 3 map images
and a `photos` list added by `generate_bimonthly_report` (the task that
actually opens the `DocxTemplate` and renders/saves it — see
`spec.yaml`'s "Patrol Report" task-group).

`spec.yaml`'s `report_template_path` / `param.yaml`'s
`report_template_path.var` point at this file.
