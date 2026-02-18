# S3 Bucket Migration Guide

## Prerequisites

-   AWS CLI installed and configured
-   Appropriate IAM permissions:
    -   `s3:ListBucket`
    -   `s3:GetObject`
    -   `s3:PutObject`
-   Network connectivity to AWS endpoints

------------------------------------------------------------------------

## Migration Steps

### 1. Create the Destination Bucket

By default, S3 buckets are private unless specific public access blocks
are removed.

``` bash
aws s3 mb s3://nautilus-sync-19683
```

------------------------------------------------------------------------

### 2. Execute Data Migration

We use the `sync` command rather than `cp`. The `sync` command is
idempotent and more efficient for large datasets as it only copies new
or updated objects.

``` bash
aws s3 sync s3://nautilus-s3-4112 s3://nautilus-sync-19683
```

**Note:**\
This command recursively navigates through all prefixes (folders) and
copies metadata along with the objects.

------------------------------------------------------------------------

### 3. Verification & Consistency Check

To confirm the migration was successful and completely transferred,
compare the object count and total size of both buckets.

#### Source Bucket

``` bash
aws s3 ls s3://nautilus-s3-4112 --recursive --summarize | tail -n 2
```

#### Destination Bucket

``` bash
aws s3 ls s3://nautilus-sync-19683 --recursive --summarize | tail -n 2
```

------------------------------------------------------------------------

## Technical Details

| Component \| Resource Name \|

\|-----------\|---------------\| Source Bucket \| nautilus-s3-4112 \|       

\|Destination Bucket \| nautilus-sync-19683 \| 
\| Primary Tool \| AWS CLI
(v2 recommended) \| \| Sync Logic \| Size and Last Modified Time
comparison \|

------------------------------------------------------------------------

## Troubleshooting

### Permissions

If you encounter **403 Forbidden** errors, ensure your IAM policy
allows: - `s3:ListBucket` and `s3:GetObject` on the source bucket -
`s3:PutObject` on the destination bucket

### Interrupted Transfers

If the process stops due to a network timeout, simply re-run the `sync`
command.\
It will resume by skipping files already present in the destination
bucket.

