# Data validation from different SQL server
```python
import pyodbc
import pandas as pd

# connect to SQL server
conn = pyodbc.connect('Driver={SQL Server};'
                      'Server=Saa01sql02;'
                      'Databse=PortalCustom;'
                      'Trusted_Connection=yes;')
                      
cursor = conn.cursor()
# Please Change Stored Procedure name when you run code.
cursor.execute('Call [sandbox].[AE_Fee_Data](?)',('11/1/2018'))
old_data = []
for row in cursor:
  old_data.append(row)
print("old DWH the number of rows: " + str(len(old_data)))

# connect to second SQL server
conn2 = pyodbc.connect('Driver={SQL Server};'
                      'Server=Saa01sql02;'
                      'Databse=PortalCustom;'
                      'Trusted_Connection=yes;')
                      
cursor2 = conn2.cursor()
# Please Change Stored Procedure name when you run code.
cursor.execute('Call [dataset].[AE_Fee_Data](?)',('11/1/2018'))
new_data = []
for row in cursor2:
  new_data.append(row)
print("new DWH the number of rows: " + str(len(new_data)))

# data comparison
diffdata =[diff for diff in new_data if diff not in old_data]
df = pd.DataFrame(diffdata)
print("unmatching data in new DWH the number of rows: "+str(len(df)))

# save result to txt file
df.to_csv(r'C:\Users\c_ssshang\Desktop\result\AE_Fee_Data.txt', sep=' ', index=False)
```

# When we want to run Sql file instead of stored procedure.
change the part of code
```python
# change this path when you run code.
fd = open(r'C:\Users\c_sshang\Desktop\new_DWH_code\Employer Data.sql','r')
sqlfile = fd.read()
fd.close()
cursor.execute(sqlfile)
```
                   
                      
                      
