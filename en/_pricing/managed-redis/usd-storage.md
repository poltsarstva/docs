{% include [local-ssd for Ice Lake only by request](../../_includes/ice-lake-local-ssd-note.md) %}

| Service                  | Rate for 1 GB per month, without VAT                      |
|--------------------------|-----------------------------------------------------------|
| Fast network storage     | {{ sku|USD|mdb.cluster.network-nvme.redis|month|string }} |
| Backup over storage size | $0.032594                                                 |
