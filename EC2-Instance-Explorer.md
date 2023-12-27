# EC2 Instance Explorer
It's important to stay continually apprised of the cloud compute requiremnets when architecting cloud solutions.  Calculatinng hourly prices to build a point model for any given project is relatively easy math, but the small configuration changes that constantly occur during the design phase can dramatically alter those calculations, which can have impact on profitability or even the feasibility of the project.  Going from on-demand to one and three year reservations brings additional complexity, exponentially so when third party licensing is part of the configuration.

The safest solution to modeling these factors is always to query live or near-live pricing data from the cloud provider (in this case AWS).  AWS's pricing model is subject to constant change, and visiting these questions frequently with live data can also bring advantages to a project, as better-fit solutions enter the AWS catalog.

## Approach
The EC2 Instance Explorer leverages the EC2 Bulk API, pulling the data into a powerbi instance running locally on the workstation or in the M365 ecosystem.  It performs some light ETL in PowerQuery/M to derive consistent filterable numeric values for values such as memory, GPU, and memory.  In DAX, helper tables are implemented to facilitate numeric filtering of Network and EBS throughput. Finally, there are simple DAX calculations to derive a three year value for each SKU, which are then rolled up and averaged.

```vb
3yr hourly = IF(
  [Unit]="Hrs",
  [PricePerUnit]*24*365.25*3,
  BLANK())

3yr upfront = IF(
  [Unit]="Quantity",
    (IF(
      [LeaseContractLength]="1yr",
      [PricePerUnit]*3,
      [PricePerUnit])),
  BLANK())

Avg Offer Price (3yr) =
  AVERAGE(ec2_instances[3yr hourly])
  +
  AVERAGE(ec2_instances[3yr upfront])
```
I primarily use this as a backend dataset for Excel queries - this is basically a clean, predictable, relatively fresh set of reference data.  But I've also published this dataset to a [dashboard](https://app.powerbi.com/view?r=eyJrIjoiYzRmOTY1MDYtZmE1ZC00MzA5LWFhMjYtMTIzM2Q0MWMwYjBlIiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9) where the user can quickly get to a right-fit solution by filtering on resource parameters, such as vCPU, memory, and network bandwith, and consumption model, such as 1 or 3 year commits with variable upfront payments.  I've also added logic that allows the user can filter out solutions lacking guaranteed EBS or Network throughput.

[![dashboard][1]][2]

[1]:  https://github.com/pgaljan/blog/assets/11296072/c5c91c7f-daf8-4d74-bfb7-0415645f2f7f
[2]:  https://app.powerbi.com/view?r=eyJrIjoiYzRmOTY1MDYtZmE1ZC00MzA5LWFhMjYtMTIzM2Q0MWMwYjBlIiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9 "Redirect to homepage"

If you use the dashboard in M365, it should refresh weekly sometime on Sunday.  I pull pricing data for US East, since that region is most likely to have all instance types, and relative prices based on location are relatively easy to derive.  I can look into adding more regions, or replacing US-East with another region.

## Next Steps
There are obvious next steps for a dashboard like this - I could add other EC2 product families like NAT gateway, egress, transfer etc, but most of my immediate efforts are being placed in building excel-based solutions that build incrementally on this dataset.  If you'd like to collaborate or If you have suggestions, please reach out.
