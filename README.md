# DOWNLOAD-DATA-FROM-AFRICAN-DEVELOPMENT-BANK
Signature:
fetch_series(
    provider_code=None,
    dataset_code=None,
    series_code=None,
    dimensions=None,
    series_ids=None,
    max_nb_series=None,
    api_base_url=None,
    editor_api_base_url='https://editor.nomics.world/api/v1/',
    filters=None,
    timeout=None,
)
Docstring:
Download time series from DBnomics. Filter series by different ways according to the given parameters.

If not `None`, `dimensions` parameter must be a `dict` of dimensions (`list` of `str`), like so:
`{"freq": ["A", "M"], "country": ["FR"]}`.

If not `None`, `series_code` must be a `str`. It can be a series code (one series), or a "mask" (many series):
- remove a constraint on a dimension, for example `M..PCPIEC_WT`;
- enumerate many values for a dimension, separated by a '+', for example `M.FR+DE.PCPIEC_WT`;
- combine these possibilities many times in the same SDMX filter.

If the rightmost dimension value code is removed, then the final '.' can be removed too: `A.FR.` = `A.FR`.

If not `None`, `series_ids` parameter must be a non-empty `list` of series IDs.
A series ID is a string formatted like `provider_code/dataset_code/series_code`.

If `max_nb_series` is `None`, a default value of 50 series will be used.

If `filters` is not `None`, apply those filters using the Time Series Editor API
(Cf https://editor.nomics.world/filters)

Return a Python Pandas `DataFrame`.

Examples
--------
- fetch one series:
  fetch_series("AMECO/ZUTN/EA19.1.0.0.0.ZUTN")

- fetch all the series of a dataset:
  fetch_series("AMECO", "ZUTN")

- fetch many series from different datasets:
  fetch_series(["AMECO/ZUTN/EA19.1.0.0.0.ZUTN", "AMECO/ZUTN/DNK.1.0.0.0.ZUTN", "IMF/CPI/A.AT.PCPIT_IX"])

- fetch many series from the same dataset, searching by dimension:
  fetch_series("AMECO", "ZUTN", dimensions={"geo": ["dnk"]})

- fetch many series from the same dataset, searching by code mask:
  fetch_series("IMF", "CPI", series_code="M.FR+DE.PCPIEC_WT")
  fetch_series("IMF", "CPI", series_code=".FR.PCPIEC_WT")
  fetch_series("IMF", "CPI", series_code="M..PCPIEC_IX+PCPIA_IX")

- fetch one series and apply interpolation filter:
  fetch_series(
      'AMECO/ZUTN/EA19.1.0.0.0.ZUTN',
      filters=[{"code": "interpolate", "parameters": {"frequency": "monthly", "method": "spline"}}],
  )
File:      c:\users\richard lu\anaconda3\lib\site-packages\dbnomics\__init__.py
Type:      function
