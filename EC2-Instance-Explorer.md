# EC2 Instance Explorer
Project success and failure often pivot on operating costs and performance, making right-sizing the compute infrastructure to fit project requirements a critical System Architect responsibility. This task carries significant impact and risk, as even minor alterations to configuration, consumption, and licensing models can significantly amplify costs. While implementing billing metrics and identifying opportunities during production is relatively straightforward, accurately estimating projects during the design or re-design phase poses greater challenges.

Architects frequently develop customized models tailored to individual projects. However, leveraging live data from AWS APIs for comprehensive 'what-if' assessments of architectural choices proves challenging. The EC2 Instance Explorer introduces an innovative approach utilizing the AWS Bulk Pricing API to collect pricing data from all EC2 instance SKUs in the US-East AWS region. It then standardizes this data to display a three-year spending projection and provides users with an online DAX data model compatible with Excel or Power BI for creating visually-driven dashboards. This tool serves as a demonstration of an approach that could be replicated not only across various AWS product families but also across other cloud platforms.

## Approach
The model pulls the data from the AWS Bulk API into a PowerBI instance running locally on the workstation or in the M365 ecosystem.  It performs some light ETL in PowerQuery/M to derive consistent filterable numeric values for values such as memory, GPU, and memory.  In DAX, helper tables are implemented to facilitate numeric filtering of Network and EBS throughput. Finally, there are simple DAX calculations to derive a three year value for each SKU, which are then rolled up and averaged:

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

The table shows a hierarchical view of skus that meet the filter requirements.  The average 3 year price is the 3 year total cost averaged across all the offers associated with the sku.  The numnber of offers corresponds to the number of different consumption models the sku is available under.  (For example, Reserved, 3-year, Partial up front, vs Reserved, 1 year, All up front could be different offers of the same sku).  

[![dashboard][1]][2]

[1]:  https://github.com/pgaljan/blog/assets/11296072/41521328-4d3f-41e0-b074-da810264a591
[2]:  https://app.powerbi.com/view?r=eyJrIjoiYzRmOTY1MDYtZmE1ZC00MzA5LWFhMjYtMTIzM2Q0MWMwYjBlIiwidCI6ImZlNGQ5NDA3LWE5NzEtNDhjMy1hOTkzLTRjMmNiOGQ2MjM4NCIsImMiOjF9 "Redirect to homepage"

Contractual filters are critical to get refined numbers.  Set as many as can be reasonably determined including:
- Lease Contract Length (On Demand, 1yr, 3 yr)
- Purchase Option (All upfront, no upfront, etc)
- Tenancy (Dedicated, shared, reserved, etc)
- Offer class (convertible, standard)
- Operating System (Windows, Linux, RHEL, etc)
- Pre-Installed SW (SQL Web, Std, Ent)

If you use the dashboard in M365, it should refresh weekly sometime on Sunday.  I pull pricing data for US East, since that region is most likely to have all instance types, and relative prices based on location are relatively easy to derive.  I can look into adding more regions, or replacing US-East with another region.

## Next Steps
There are obvious next steps for a dashboard like this - I could add other EC2 product families like NAT gateway, egress, transfer etc, but most of my immediate efforts are being placed in building excel-based solutions that build incrementally on this dataset.  If you'd like to collaborate or If you have suggestions, please feel free to [contribute](https://github.com/pgaljan/EC2-Instance-Explorer) and reach out.
