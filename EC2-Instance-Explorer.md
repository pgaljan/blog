# EC2 Instance Explorer
It's important to stay continually apprised of the cloud compute requiremnets when architecting cloud solutions.  Calculatinng hourly prices to build a point model for any given project is relatively easy math, but the small configuration changes that constantly occur during the design phase can dramatically alter those calculations, which can have impact on profitability or even the feasibility of the project.  Going from on-demand to one and three year reservations adds mroe complexity, exponentially so when third party licensing is part of the configuration.

Additionally, given the constantly evolving nature of the AWS catalog and pricing model, the safest solution is always to query live or near-live pricing data from the cloud provider (in this case AWS).  Visiting these questions frequently can also bring advantages to a project, as AWS brings new solutions to the catalog that may be a closer fit to needs as they evolve.

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
  AVERAGE(ec2_instances[3yr hourly]) + AVERAGE(ec2_instances[3yr upfront])
```
The resulting dashboard, the user can filter the entire US-East Catalog based on a handful of critical resource parameters, such as vCPU, memory, and network bandwith.  Additionally, the user can filter to those skus which have guaranteed EBS or Network throughput.

and should refresh weekly.  I pull pricing data for US East, as it's most likely to have all instance types, and relative prices based on location are relatively easy to derive.

## Next Steps
-

(I could change regions, or add others if there's a good reason, so please let me know if it would be helpful, and I'll add it to the backlog).

It is designed to be leveraged via Excel query and paired with other data sources and sizers, however, there are several oppotunities to leverage this data for broad analysis and visualization.
