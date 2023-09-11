# Getting started
- [Go to Report](#report)
   - [Report From std Form](#report-from-std-form)
   - [Create Report](#create-report)
   - [Report New Form (Criteria Form)](#report-new-form-criteria-form)

# Report

> ### **Report From std Form**

1. Customization Projects
   ![image](./images/reports/Customization_Project.png)

2. Customization Project Editor > Screens > Customize Existing Screen > เลือก screen ที่ต้องการแก้ไข
   ![image](./images/reports/Customization_Project_Editor.png)

3. เลือก Customize Business Logic
   ![image](./images/reports/Business_Logic.png)
   ![image](./images/reports/Business_Logic_Code.png)

4. วางโค้ดด้านล่างลงไป (เพิ่มปุ่มลิงค์ไปที่หน้ารีพอร์ต โดยจะส่งค่า parameters จากหน้าฟอร์มไปรีพอร์ต)

   - PXAction\<ParentDAC\> **ดูจาก Data Class ของ Form (Ctrl+Alt+Left Click)**
   - ex = PXReportRequiredException.CombineReport(ex, **Screen ID ของ Report**, **parameters ของ Report**);

   ```c#
   #region Action BillingReport
       public PXAction<ErppARBilling> BillingReport;
       [PXButton(CommitChanges = true, SpecialType = PXSpecialButtonType.Report)]
       [PXUIField(DisplayName = "ERPP AR Billing Report", MapEnableRights = PXCacheRights.Select)]
       protected virtual IEnumerable billingReport(PXAdapter adapter)
       {
           PXReportRequiredException ex = null;
           foreach(ErppARBilling doc in adapter.Get<ErppARBilling>())
           {
               var parameters = new Dictionary<string, string>();

               parameters["RefNbr"] = doc.RefNbr;
               ex = PXReportRequiredException.CombineReport(ex, "BI200101", parameters);
           }
           if (ex != null) throw ex;

           return adapter.Get();
       }
       #endregion
   ```

   ![image](./images/reports/Code_Report_Put.png)
   Parent DAC
   ![image](./images/reports/ParentDAC.png)
   Set Category ผ่านทาง constrctor ของ class นั้น

   ```c#
   public ErppBillingMaint()
      {
          this.RHold.SetConnotation(ActionConnotation.Success);
          this.BillingReport.SetCategory("Reports");
          this.BillingReport.SetEnabled(false);
      }
   ```

5. Publish > Publish Current Project
   ![image](./images/reports/Publish.png)

6. Result
   ![image](./images/reports/Button_Report.png)

> ### **Create Report**

1. Open Report Desginer
   ![image](./images/reports/Report_Designer.png)

2. File > Build schema...

   Table
   ![image](./images/reports/Report_Designer_Build_Schema.png)
   Relationships
   ![image](./images/reports/Report_Designer_Build_Schema_RelationShip.png)
   Parameters (View Name จะแสดง Data Selector สามารถกำหนดได้จากฟิลด์ของ DAC ที่มี Selector Attributes)
   ![image](./images/reports/Report_Designer_Build_Schema_Parameters.png)
   Filters
   ![image](./images/reports/Report_Designer_Build_Schema_Filters.png)

3. File > Save to Server
   ![image](./images/reports/Report_Designer_Save.png)
   กด Load reports list เช็คชื่อว่ามีหรือซ้ำไหม
   ![image](./images/reports/Report_Designer_Save_Name.png)

4. Open Site Map

- กำหนด Screen ID
- กำหนด Title
- Url ให้ ID เท่ากับชื่อไฟล์รีพอร์ตที่เราตั้งไว้
- WorkSpaces และ Category คือการกำหนดที่อยู่รีพอร์ต
  ![image](./images/reports/Site_Map_Report.png)

5. Save

6. หน้าเรีอกรีพอร์ต
   ![image](./images/reports/Report_Luanch.png)
   ![image](./images/reports/Report_Luanch_Result.png)

- ตัวอย่างลิงค์มากกว่าหนึ่งรีพอร์ต

```c#
    #region Action Billing Summary Report
        public PXAction<ErppARBilling> BillingSummaryReport;
        [PXButton(CommitChanges = true, SpecialType = PXSpecialButtonType.Report)]
        [PXUIField(DisplayName = "ERPP AR Billing Summary Report", MapEnableRights = PXCacheRights.Select)]
        protected virtual IEnumerable billingSummaryReport(PXAdapter adapter)
        {
            return adapter.Get();
        }
        #endregion
```

```c#
  public ErppBillingMaint()
      {
          this.RHold.SetConnotation(ActionConnotation.Success);
          this.BillingReport.SetCategory("Reports");
          this.BillingReport.SetEnabled(false);

          this.BillingSummaryReport.SetCategory("Reports");
      }
```

![image](./images/reports/Report_Category_Example_Btn.png)

> ### **Report New Form (Criteria Form)**

1. ex report (so641010.rpx)
   ![image](./images/reports/Ex_new_1.png)

2. File > Build schema > Parameters
   ![image](./images/reports/Ex_new_2.png)
   View Name
   ![image](./images/reports/Ex_new_3.png)

   - Report.GetFieldSchema('DAC.Field')
     - (ฟิลลืที่มี Selector Attributes เช่น Order Type, Order Nbr)
       ![image](./images/reports/Ex_new_4.png)
       ![image](./images/reports/Ex_new_5.png)

3. filter from parameters
   ![image](./images/reports/Ex_new_6.png)

4. File > Save To Server > Rename (ex641010) > Ok
   ![image](./images/reports/Ex_new_7.png)

5. site map > Add Row
   ![image](./images/reports/Ex_new_8.png)
   set properties

   - Screen ID
   - Title
   - URL (ID เท่ากับชื่อรีพอร์ต เช่น ex641010.rpx)
     ![image](./images/reports/Ex_new_9.png)

6. Search Screen
   ![image](./images/reports/Ex_new_10.png)

7. result
   ![image](./images/reports/Ex_new_11.png)

> ### **Additional Filtering Conditions**

1. Report Designer > File > Build Schema > Viewer Fields > Load used fields
   ![image](./images/reports/Additional_Filter_2.png)

2. result
   ![image](./images/reports/Additional_Filter_1.png)