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

##### Concepts Source: https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
