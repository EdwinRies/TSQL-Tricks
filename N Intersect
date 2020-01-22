-- DEFINING A SCHEMA TO EXPLAIN THE EXAMPLE
CREATE TABLE A(
    ID INT PRIMARY KEY IDENTITY(1,1),
    NAME VARCHAR(50)
  )
  
GO

INSERT INTO A (NAME)
SELECT 'FOO'
UNION ALL
SELECT 'BAR'
UNION ALL
SELECT 'BAZ'

GO

CREATE TABLE B(
    ID INT PRIMARY KEY IDENTITY(1,1),
    A_ID INT FOREIGN KEY REFERENCES A(ID),
    NAME VARCHAR(50)
)

GO

INSERT INTO B(A_ID, NAME)
SELECT 1, 'APPLE'
UNION ALL
SELECT 2, 'APPLE'
UNION ALL
SELECT 3, 'APPLE'
UNION ALL
SELECT 1, 'MANGO'
UNION ALL
SELECT 2, 'MANGO'
UNION ALL
SELECT 1, 'BANANA'
UNION ALL
SELECT 3, 'BANANA'
UNION ALL
SELECT 2, 'ORANGE'
UNION ALL
SELECT 3, 'ORANGE'

-- END OF SCHEMA CREATION



;WITH CTE AS (
  SELECT
    A.ID A_ID, -- For the participating entities of A
    B.NAME B_NAME -- What is the common relating B NAME
  FROM A
  INNER JOIN B
        ON B.A_ID = A.ID  
  WHERE A.NAME IN ('FOO', 'BAZ') -- Define participants in intersect of A
  
  -- You should not need a GROUP BY unless you have duplicates of A-B combinations
  GROUP BY A.ID, B.NAME
)
SELECT B_NAME
FROM CTE MAIN
CROSS APPLY(
    SELECT COUNT(DISTINCT A_ID) C FROM CTE A
) A -- Get the amount of participants
CROSS APPLY(
    SELECT COUNT(B_NAME) C FROM CTE B WHERE B.B_NAME = MAIN.B_NAME
) B -- How many participants is referenced by this value
WHERE A.C = B.C -- if the amount of participants is equal to the 
                -- amount containing the value, we know they each have it in common
GROUP BY B_NAME -- We want the common values grouped for a distinct list
