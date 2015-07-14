# C# MVC Winform Training Material
This is the training material for the C# MVC Winform

## Model
A model stores data that is retrieved to the controller and displayed in the view. Whenever there is a change to the data it is updated by the controller.

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
<br />
## View
A view requests information from the model that it uses to generate an output representation to the user.
<br />
##### Concepts Source: https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
