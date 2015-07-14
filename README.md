# C# MVC Material
This is the training material for the C# MVC Winform

## MVC Introduction
The idea behind the Model-View-Controller architectural pattern is simple: we must have the following responsibilities clearly separated in our application:

<img src="https://s3.amazonaws.com/nettuts/613_mvc/components.jpg" />

## Model
A model stores data that is retrieved to the controller and displayed in the view. Whenever there is a change to the data it is updated by the controller.

It is the data and the rules applying to that data, which represent concepts that the application manages. In any software system, everything is modeled as data that we handle in a certain way. What is a user, a message or a book for an application? Only data that must be handled according to specific rules (date can not be in the future, e-mail must have a specific format, name cannot be more than x characters long, etc).

<img src="https://s3.amazonaws.com/nettuts/613_mvc/ModelData.jpg" />

The model gives the controller a data representation of whatever the user requested (a message, a list of books, a photo album, etc). This data model will be the same no matter how we may want to present it to the user, that's why we can choose any available view to render it.

The model contains the most important part of our application logic, the logic that applies to the problem we are dealing with (a forum, a shop, a bank, etc). The controller contains a more internal-organizational logic for the application itself (more like housekeeping).

### Model Introduction
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

using FIT.SQL;
using FIT.Methods;
using System.Data;
using System.Data.SqlClient;
using FIT.Methods;

namespace Models
{
    // Financial Statement
    public class FinancialStatement : DBConn
    {
        public string HasEntries { get; set; }
        public string Account { get; set; }
        public string CoCd { get; set; }
        public string ProfitCtr { get; set; }
        public string Plnt { get; set; }
        public string Material { get; set; }
        public string Assignment { get; set; }
        public string DocumentNo { get; set; }
        public string Type { get; set; }
        public string Pe { get; set; }
        public string PK { get; set; }
        public string PstngDate { get; set; }
        public string Curr { get; set; }
        public double Quantity { get; set; }
        public double AmountLC { get; set; }
        public double AmountLC2 { get; set; }
        public string DocDate { get; set; }
        public string ShortName { get; set; }
        // Active Record Properties
        public string ID { get; set; }
        public clsSQL SQL { get { return sql; } }
        public string Table { get { return "tbl_FinancialStatement"; } }

        public FinancialStatement()
        {

        }

        public FinancialStatement(string Account, string CoCd, string ProfitCtr, string Plnt, string Material, string Assignment,
                  string DocumentNo, string Type, string Pe, string PK, string PstngDate, string Curr, double Quantity, double AmountLC,
                  double AmountLC2, string DocDate, string ShortName, string ID, string HasEntries)
        {
            this.HasEntries = HasEntries;
            this.Account = Account;
            this.CoCd = CoCd;
            this.ProfitCtr = ProfitCtr;
            this.Plnt = Plnt;
            this.Material = Material;
            this.Assignment = Assignment;
            this.DocumentNo = DocumentNo;
            this.Type = Type;
            this.Pe = Pe;
            this.PK = PK;
            this.PstngDate = PstngDate;
            this.Curr = Curr;
            this.Quantity = Quantity;
            this.AmountLC = AmountLC;
            this.AmountLC2 = AmountLC2;
            this.DocDate = DocDate;
            this.ShortName = ShortName;
            this.ID = ID;
        }

        public void uploadFS(DataTable dtFS)
        {
            try
            {
                clsMethodsGlobal gblMethd = new clsMethodsGlobal();
                dtFS.Columns.Add("ShortName");
                dtFS.Columns.Add("ID");
                foreach (DataRow drRow in dtFS.Rows)
                {
                    drRow["ShortName"] = Environment.UserName;
                    drRow["ID"] = gblMethd.getID();
                }

                // Delete User Data
                sql.executeQuery("DELETE FROM tbl_FinancialStatement WHERE ShortName = @ShortName",
                                new SqlParameter[]{ new SqlParameter("@ShortName", Environment.UserName) });

                // Upload User Data
                sql.bulk(dtFS, "tbl_FinancialStatement");
            }
            catch (Exception ex)
            {
                throw ex;
            }
        }

        #region "MANUAL CRUD"

        public List<FinancialStatement> select()
        {
            try
            {
                DataTable dtFS = sql.fillDataTable("SELECT * FROM tbl_FinancialStatement WHERE ShortName = @ShortName",
                                                    new SqlParameter[] { new SqlParameter("@ShortName", Environment.UserName) });

                List<FinancialStatement> lstFS = new List<FinancialStatement>();

                foreach (DataRow drRow in dtFS.Rows)
                {
                    FinancialStatement clsFS = new FinancialStatement {
                        Account = drRow["Account"].ToString(),
                        CoCd = drRow["CoCd"].ToString(),
                        ProfitCtr = drRow["ProfitCtr"].ToString(),
                        Plnt = drRow["Plnt"].ToString(),
                        Material = drRow["Material"].ToString(),
                        Assignment = drRow["Assignment"].ToString(),
                        DocumentNo = drRow["DocumentNo"].ToString(),
                        Type = drRow["Type"].ToString(),
                        Pe = drRow["Pe"].ToString(),
                        PK = drRow["PK"].ToString(),
                        PstngDate = drRow["PstngDate"].ToString(),
                        Curr = drRow["Curr"].ToString(),
                        Quantity = float.Parse(drRow["Quantity"].ToString()),
                        AmountLC = float.Parse(drRow["AmountLC"].ToString()),
                        AmountLC2 = float.Parse(drRow["AmountLC2"].ToString()),
                        DocDate = drRow["DocDate"].ToString(),
                        ShortName = drRow["ShortName"].ToString(),
                        ID = drRow["ID"].ToString(),
                        HasEntries = drRow["HasEntries"].ToString(),
                    };

                    lstFS.Add(clsFS);
                }

                return lstFS;
            }
            catch (Exception ex)
            { throw ex; }
        }

        public int insert(string Account, string CoCd, string ProfitCtr, string Plnt, string Material, string Assignment,
                  string DocumentNo, string Type, string Pe, string PK, string PstngDate, string Curr, double Quantity, double AmountLC,
                  double AmountLC2, string DocDate, string HasEntries)
        {
            try
            {
                clsMethodsGlobal clsMethods = new clsMethodsGlobal();

                return sql.executeQuery("INSERT INTO tbl_FinancialStatement VALUES " +
                                        "(@Account, @CoCd, @ProfitCtr, @Plnt, @Material, @Assignment, @DocumentNo, @Type, @Pe, @PK, " +
                                        "@PstngDate, @Curr, @Quantity, @AmountLC, @AmountLC2, @DocDate, @ShortName, @ID, @HasEntries)",
                                        new SqlParameter[] { 
                                            new SqlParameter("@Account", Account),
                                            new SqlParameter("@CoCd", CoCd),
                                            new SqlParameter("@ProfitCtr", ProfitCtr),
                                            new SqlParameter("@Plnt", Plnt),
                                            new SqlParameter("@Material", Material),
                                            new SqlParameter("@Assignment", Assignment),
                                            new SqlParameter("@DocumentNo", DocumentNo),
                                            new SqlParameter("@Type", Type),
                                            new SqlParameter("@Pe", Pe),
                                            new SqlParameter("@PK", PK),
                                            new SqlParameter("@PstngDate", PstngDate),
                                            new SqlParameter("@Curr", Curr),
                                            new SqlParameter("@Quantity", Quantity),
                                            new SqlParameter("@AmountLC", AmountLC),
                                            new SqlParameter("@AmountLC2", AmountLC2),
                                            new SqlParameter("@DocDate", DocDate),
                                            new SqlParameter("@ShortName", Environment.UserName),
                                            new SqlParameter("@HasEntries", HasEntries),
                                            new SqlParameter("@ID", clsMethods.getID())
                                        });

            }
            catch (Exception ex)
            { throw ex; }
        }


        public int update(string Account, string CoCd, string ProfitCtr, string Plnt, string Material, string Assignment,
                          string DocumentNo, string Type, string Pe, string PK, string PstngDate, string Curr, double Quantity, double AmountLC,
                          double AmountLC2, string DocDate, string ShortName, string HasEntries, string ID)
        {
            try
            {
                return sql.executeQuery("UPDATE tbl_FinancialStatement SET " +
                                        "Account = @Account, CoCd= @CoCd, ProfitCtr = @ProfitCtr, Plnt = @Plnt, Material = @Material, " +
                                        "Assignment = @Assignment, DocumentNo = @DocumentNo, Type = @Type, Pe = @Pe, PK = @PK, " +
                                        "PstngDate = @PstngDate, Curr = @Curr, Quantity = @Quantity, AmountLC = @AmountLC, " +
                                        "AmountLC2 = @AmountLC2, DocDate = @DocDate, ShortName = @ShortName, HasEntries = @HasEntries " +
                                        "WHERE ID = @ID",
                                        new SqlParameter[] { 
                                            new SqlParameter("@Account", Account),
                                            new SqlParameter("@CoCd", CoCd),
                                            new SqlParameter("@ProfitCtr", ProfitCtr),
                                            new SqlParameter("@Plnt", Plnt),
                                            new SqlParameter("@Material", Material),
                                            new SqlParameter("@Assignment", Assignment),
                                            new SqlParameter("@DocumentNo", DocumentNo),
                                            new SqlParameter("@Type", Type),
                                            new SqlParameter("@Pe", Pe),
                                            new SqlParameter("@PK", PK),
                                            new SqlParameter("@PstngDate", PstngDate),
                                            new SqlParameter("@Curr", Curr),
                                            new SqlParameter("@Quantity", Quantity),
                                            new SqlParameter("@AmountLC", AmountLC),
                                            new SqlParameter("@AmountLC2", AmountLC2),
                                            new SqlParameter("@DocDate", DocDate),
                                            new SqlParameter("@ShortName", ShortName),
                                            new SqlParameter("@HasEntries", HasEntries),
                                            new SqlParameter("@ID", ID)
                                        });

            }
            catch (Exception ex)
            { throw ex; }
        }

        public int delete(string ID)
        {
            try
            {
                return sql.executeQuery("DELETE FROM tbl_FinancialStatement " +
                                        "WHERE ID = @ID",
                                        new SqlParameter[] { new SqlParameter("@ID", ID) });
            }
            catch (Exception ex)
            { throw ex; }
        }

        #endregion

    }
}
```
<br>
### Difference between Manual CRUD versus ActiveRecord CRUD

#### Model - ( Create / Insert )
```cs
//                                            >> MODEL - MANUAL EXAMPLE <<
// Imports
using FIT.Methods;

// Create / Insert
public int insert(string Account, string CoCd, string ProfitCtr, string Plnt, string Material, string Assignment,
                  string DocumentNo, string Type, string Pe, string PK, string PstngDate, string Curr, double Quantity, double AmountLC,
                  double AmountLC2, string DocDate, string HasEntries)
        {
            try
            {
                clsMethodsGlobal clsMethods = new clsMethodsGlobal();

                return sql.executeQuery("INSERT INTO tbl_FinancialStatement VALUES " +
                                        "(@Account, @CoCd, @ProfitCtr, @Plnt, @Material, @Assignment, @DocumentNo, @Type, @Pe, @PK, " +
                                        "@PstngDate, @Curr, @Quantity, @AmountLC, @AmountLC2, @DocDate, @ShortName, @ID, @HasEntries)",
                                        new SqlParameter[] { 
                                            new SqlParameter("@Account", Account),
                                            new SqlParameter("@CoCd", CoCd),
                                            new SqlParameter("@ProfitCtr", ProfitCtr),
                                            new SqlParameter("@Plnt", Plnt),
                                            new SqlParameter("@Material", Material),
                                            new SqlParameter("@Assignment", Assignment),
                                            new SqlParameter("@DocumentNo", DocumentNo),
                                            new SqlParameter("@Type", Type),
                                            new SqlParameter("@Pe", Pe),
                                            new SqlParameter("@PK", PK),
                                            new SqlParameter("@PstngDate", PstngDate),
                                            new SqlParameter("@Curr", Curr),
                                            new SqlParameter("@Quantity", Quantity),
                                            new SqlParameter("@AmountLC", AmountLC),
                                            new SqlParameter("@AmountLC2", AmountLC2),
                                            new SqlParameter("@DocDate", DocDate),
                                            new SqlParameter("@ShortName", Environment.UserName),
                                            new SqlParameter("@HasEntries", HasEntries),
                                            new SqlParameter("@ID", clsMethods.getID())
                                        });

            }
            catch (Exception ex)
            { throw ex; }
        }
        
//                                        >> MODEL - ACTIVE RECORD EXAMPLE <<
// Imports
using FIT.ActiveRecord;
using FIT.Methods;
using Models;

// Create / Insert
clsMethodsGlobal clsMethods = new clsMethodsGlobal();

FinancialStatement fs = 
              new FinancialStatement("5698012A3A", "A26", "C516600232", "A2SD3", "M565ADASD", 
                                     "A2321SDSS", "DOC265558", "T86", "P12A", "PK25", "2015-05-05", 
                                     "USD", 21.5, 30.5, 45.6, "2015-06-07", Environment.UserName, clsMethods.getID(), null);
```
<br />
#### Model - ( Read / Select )
```cs
//                                            >> MODEL - MANUAL EXAMPLE <<
// Imports
using FIT.Methods;

// Read / Select
public List<FinancialStatement> select()
        {
            try
            {
                DataTable dtFS = sql.fillDataTable("SELECT * FROM tbl_FinancialStatement WHERE ShortName = @ShortName",
                                                    new SqlParameter[] { new SqlParameter("@ShortName", Environment.UserName) });

                List<FinancialStatement> lstFS = new List<FinancialStatement>();

                foreach (DataRow drRow in dtFS.Rows)
                {
                    FinancialStatement clsFS = new FinancialStatement {
                        Account = drRow["Account"].ToString(),
                        CoCd = drRow["CoCd"].ToString(),
                        ProfitCtr = drRow["ProfitCtr"].ToString(),
                        Plnt = drRow["Plnt"].ToString(),
                        Material = drRow["Material"].ToString(),
                        Assignment = drRow["Assignment"].ToString(),
                        DocumentNo = drRow["DocumentNo"].ToString(),
                        Type = drRow["Type"].ToString(),
                        Pe = drRow["Pe"].ToString(),
                        PK = drRow["PK"].ToString(),
                        PstngDate = drRow["PstngDate"].ToString(),
                        Curr = drRow["Curr"].ToString(),
                        Quantity = float.Parse(drRow["Quantity"].ToString()),
                        AmountLC = float.Parse(drRow["AmountLC"].ToString()),
                        AmountLC2 = float.Parse(drRow["AmountLC2"].ToString()),
                        DocDate = drRow["DocDate"].ToString(),
                        ShortName = drRow["ShortName"].ToString(),
                        ID = drRow["ID"].ToString(),
                        HasEntries = drRow["HasEntries"].ToString(),
                    };

                    lstFS.Add(clsFS);
                }

                return lstFS;
            }
            catch (Exception ex)
            { throw ex; }
        }

//                                        >> MODEL - ACTIVE RECORD EXAMPLE <<
// Imports
using FIT.ActiveRecord;
using Models;

FinancialStatement fs = new FinancialStatement();
List<FinancialStatement> lstFS =  
      fs.ShowWhere("ShortName = @ShortName", 
      new List<clsParam>() { new clsParam("@ShortName", Environment.UserName) });
```
<br />
#### Model - ( Update )
```cs
//                                            >> MODEL - MANUAL EXAMPLE <<
// Imports
using FIT.Methods;

// Update
public int update(string Account, string CoCd, string ProfitCtr, string Plnt, string Material, string Assignment,
                          string DocumentNo, string Type, string Pe, string PK, string PstngDate, string Curr, double Quantity, double AmountLC,
                          double AmountLC2, string DocDate, string ShortName, string HasEntries, string ID)
        {
            try
            {
                return sql.executeQuery("UPDATE tbl_FinancialStatement SET " +
                                        "Account = @Account, CoCd= @CoCd, ProfitCtr = @ProfitCtr, Plnt = @Plnt, Material = @Material, " +
                                        "Assignment = @Assignment, DocumentNo = @DocumentNo, Type = @Type, Pe = @Pe, PK = @PK, " +
                                        "PstngDate = @PstngDate, Curr = @Curr, Quantity = @Quantity, AmountLC = @AmountLC, " +
                                        "AmountLC2 = @AmountLC2, DocDate = @DocDate, ShortName = @ShortName, HasEntries = @HasEntries " +
                                        "WHERE ID = @ID",
                                        new SqlParameter[] { 
                                            new SqlParameter("@Account", Account),
                                            new SqlParameter("@CoCd", CoCd),
                                            new SqlParameter("@ProfitCtr", ProfitCtr),
                                            new SqlParameter("@Plnt", Plnt),
                                            new SqlParameter("@Material", Material),
                                            new SqlParameter("@Assignment", Assignment),
                                            new SqlParameter("@DocumentNo", DocumentNo),
                                            new SqlParameter("@Type", Type),
                                            new SqlParameter("@Pe", Pe),
                                            new SqlParameter("@PK", PK),
                                            new SqlParameter("@PstngDate", PstngDate),
                                            new SqlParameter("@Curr", Curr),
                                            new SqlParameter("@Quantity", Quantity),
                                            new SqlParameter("@AmountLC", AmountLC),
                                            new SqlParameter("@AmountLC2", AmountLC2),
                                            new SqlParameter("@DocDate", DocDate),
                                            new SqlParameter("@ShortName", ShortName),
                                            new SqlParameter("@HasEntries", HasEntries),
                                            new SqlParameter("@ID", ID)
                                        });

            }
            catch (Exception ex)
            { throw ex; }
        }

//                                        >> MODEL - ACTIVE RECORD EXAMPLE <<
// Imports
using FIT.ActiveRecord;
using Models;

FinancialStatement fs =
    new FinancialStatement("5698012A3A", "A26", "C516600232", "A2SD3", "M565ADASD",
                           "A2321SDSS", "DOC265558", "T86", "P12A", "PK25", "2015-05-05",
                           "USD", 21.5, 30.5, 45.6, "2015-06-07", Environment.UserName, "SPECIFICID", "Yes");
fs.Update();
```
<br />
#### Model - ( Delete )
```cs
//                                            >> MODEL - MANUAL EXAMPLE <<
// Imports
using FIT.Methods;

// Delete
public int delete(string ID)
        {
            try
            {
                return sql.executeQuery("DELETE FROM tbl_FinancialStatement " +
                                        "WHERE ID = @ID",
                                        new SqlParameter[] { new SqlParameter("@ID", ID) });
            }
            catch (Exception ex)
            { throw ex; }
        }

//                                        >> MODEL - ACTIVE RECORD EXAMPLE <<
// Imports
using FIT.ActiveRecord;
using Models;

FinancialStatement fs = new FinancialStatement();
fs.ID = "SPECIFICID";
fs.Delete();
```
<br />
## Controller
A controller can send commands to the model to update the model's state (e.g., editing a document). It can also send commands to its associated view to change the view's presentation of the model (e.g., by scrolling through a document).

It manages the user requests (received as HTTP GET or POST requests when the user clicks on GUI elements to perform actions). Its main function is to call and coordinate the necessary resources/objects needed to perform the user action. Usually the controller will call the appropriate model for the task and then selects the proper view.

<br />
## View
The View provides different ways to present the data received from the model. They may be templates where that data is filled. There may be several different views and the controller has to decide which one to use.

A web application is usually composed of a set of controllers, models and views. The controller may be structured as a main controller that receives all requests and calls specific controllers that handles actions for each case.
<br />
<br />
<br />
###### <i>MVC Concepts' Source</i>
http://code.tutsplus.com/tutorials/mvc-for-noobs--net-10488
https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
