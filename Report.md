# REPORT

> **เพิ่มรายงานที่ form**

1. Customization Projects
   ![image](./images/Customization_Project.png)

2. Customization Project Editor > Screens > Customize Existing Screen > เลือก screen ที่ต้องการแก้ไข
   ![image](./images/Customization_Project_Editor.png)

3. เลือก Customize Business Logic
   ![image](./images/Business_Logic.png)
   ![image](./images/Business_Logic_Code.png)

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

   ![image](./images/Code_Report_Put.png)
   Parent DAC
   ![image](./images/ParentDAC.png)
   Set Category ผ่านทาง constrctor ของ class นั้น
   ![image](./images/Code_Report_Put_Ctor.png)

5. Publish > Publish Current Project
   ![image](./images/Publish.png)

6. Result
   ![image](./images/Button_Report.png)

> **สร้างรายงาน**

1. Open Report Desginer
   ![image](./images/Report_Designer.png)

2. File > Build schema...

   Table
   ![image](./images/Report_Designer_Build_Schema.png)
   Relationships
   ![image](./images/Report_Designer_Build_Schema_RelationShip.png)
   Parameters (View Name จะแสดง Data Selector สามารถกำหนดได้จากฟิลด์ของ DAC ที่มี Selector Attributes)
   ![image](./images/Report_Designer_Build_Schema_Parameters.png)
   Filters
   ![image](./images/Report_Designer_Build_Schema_Filters.png)

3. File > Save to Server
   ![image](./images/Report_Designer_Save.png)
   กด Load reports list เช็คชื่อว่ามีหรือซ้ำไหม
   ![image](./images/Report_Designer_Save_Name.png)

4. Open Site Map

- กำหนด Screen ID
- กำหนด Title
- Url ให้ ID เท่ากับชื่อไฟล์รีพอร์ตที่เราตั้งไว้
- WorkSpaces และ Category คือการกำหนดที่อยู่รีพอร์ต
  ![image](./images/Site_Map_Report.png)

5. Save

6. หน้าเรีอกรีพอร์ต
   ![image](./images/Report_Luanch.png)
   ![image](./images/Report_Luanch_Result.png)

- ตัวอย่างลิงค์มากกว่าหนึ่งรีพอร์ต
  ![image](./images/Report_Category_Example.png)
  ![image](./images/Report_Category_Example_Ctor.png)
  ![image](./images/Report_Category_Example_Btn.png)
