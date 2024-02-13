IDENTIFICATION DIVISION.
PROGRAM-ID. SELECT_BACKUP_DELETE.
ENVIRONMENT DIVISION.
INPUT-OUTPUT SECTION.
DATA DIVISION.
WORKING-STORAGE SECTION.
01  CONTROL-NUMBER-IN   PIC X(10).
01  CONTROL-NUMBER-DB   PIC X(10).
01  FIELD-1             PIC X(20).
01  FIELD-2             PIC X(30).
01  FIELD-3             PIC X(40).
01  SQLCODE             PIC S9(9) COMP.
EXEC SQL INCLUDE SQLCA END-EXEC.
PROCEDURE DIVISION.
MAIN-PARAGRAPH.
    DISPLAY "Enter Control Number: ".
    ACCEPT CONTROL-NUMBER-IN.

    EXEC SQL
        CONNECT TO your_database_name
    END-EXEC.

    IF SQLCODE = 0
    THEN
        EXEC SQL
            DECLARE CURSOR1 CURSOR FOR
            SELECT CONTROL_NUMBER, FIELD1, FIELD2, FIELD3
            FROM YOUR_TABLE_NAME
            WHERE CONTROL_NUMBER = :CONTROL-NUMBER-IN
            FETCH FIRST 1 ROW ONLY
        END-EXEC.

        IF SQLCODE = 0
        THEN
            EXEC SQL
                OPEN CURSOR1
            END-EXEC.

            EXEC SQL
                FETCH CURSOR1 INTO
                    :CONTROL-NUMBER-DB, :FIELD-1, :FIELD-2, :FIELD-3
            END-EXEC.

            IF SQLCODE = 0
            THEN
                EXEC SQL
                    INSERT INTO BACKUP_TABLE
                    VALUES (:CONTROL-NUMBER-DB, :FIELD-1, :FIELD-2, :FIELD-3)
                END-EXEC.

                IF SQLCODE = 0
                THEN
                    EXEC SQL
                        DELETE FROM YOUR_TABLE_NAME
                        WHERE CONTROL_NUMBER = :CONTROL-NUMBER-IN
                    END-EXEC.

                    IF SQLCODE = 0
                    THEN
                        DISPLAY "Record backed up and deleted successfully."
                    ELSE
                        DISPLAY "Error in deleting record."
                    END-IF.

                ELSE
                    DISPLAY "Error in backing up record."
                END-IF.

            ELSE
                DISPLAY "No record found for Control Number: " CONTROL-NUMBER-IN
            END-IF.

            EXEC SQL
                CLOSE CURSOR1
            END-EXEC.

        ELSE
            DISPLAY "Error in fetching record."
        END-IF.

        EXEC SQL
            DISCONNECT
        END-EXEC.

    ELSE
        DISPLAY "Error in connecting to the database."
    END-IF.

    STOP RUN.
