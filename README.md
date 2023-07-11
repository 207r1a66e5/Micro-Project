# Micro-Project
## Student Enrollment Form
-Create a form based on any one of the TOPICS given below. The form should store data in the database. The primary key and input fields of each topic is mentioned.
-There will be three control buttons [Save], [Update] and [Reset] at the bottom of the form. On page load or any control button click, an empty form will be displayed and 
 the cursor will remain at the first input field in the form which will have the primary key in the relation. All other fields and buttons should be disabled at this time.
-User will enter data in the field having primary key and
-If the primary key value does NOT exist in the database, enable [Save] and [Reset] buttons and move the cursor to the next field and allow the user to enter data in the form.
-Check that the data should be valid i.e. no empty fields.
-Complete the data entry form and click the [Save] button to store the data in the database and go to step-2.
-If the primary key value is present in the database, display that data in the form. Enable [Update] and [Reset] buttons and move the cursor to the next' field in the 
 form. Keep the primary key field disabled and allow users to change other form fields.
-Check that the data should be valid i.e. no empty fields.
-Click on [Update] button to update the data in the database and go to step-2.
-Click [Reset] to reset the form as per the step-2.
-Micro Project Topics (Choose any one from following):

Student Enrollment Form that will store data in STUDENT-TABLE relation of SCHOOL-DB database.
-Input Fields: {Roll-No, Full-Name, Class, Birth-Date, Address, Enrollment-Date}
-Primary key: Roll No.
CODE:
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Micro Project Work</title>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <link
            rel="stylesheet"
            href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
        <script src="http://login2explore.com/jpdb/resources/js/0.0.3/jpdb-commons.js"></script>

    </head>
    <body>
        <div class="container">
            <h2>Student Enrollment Form (Micro Project Work)</h2>
            <form id="stuForm" method="post">
                <div class="form-group">
                    <span
                        ><label for="stuId">Roll-No:</label> <label id="stuIdMsg"> </label
                        ></span>

                    <input
                        type="text"
                        class="form-control"
                        onchange="getStu()"
                        name="stuId"
                        id="stuId"
                        placeholder="Enter Roll-No"
                        required
                        />
                </div>

                <div class="form-group">
                    <label for="stuName">Student Full Name:</label>
                    <input
                        type="text"
                        class="form-control"
                        id="stuName"
                        placeholder="Enter Full Name"
                        name="stuName"
                        />
                </div>

                <div class="form-group">
                    <label for="stuClass">Class:</label>
                    <input
                        type="text"
                        class="form-control"
                        id="stuClass"
                        placeholder="Enter Class"
                        name="stuClass"
                        />
                </div>

                <div class="form-group">
                    <label for="stuDOB">Birth-Date:</label>
                    <input
                        type="date"
                        class="form-control"
                        id="stuDOB"
                        placeholder="Enter Birth-Date"
                        name="stuDOB"
                        />
                </div>

                <div class="form-group">
                    <label for="stuAddress">Address:</label>
                    <input
                        type="text"
                        class="form-control"
                        id="stuAddress"
                        placeholder="Enter Address"
                        name="stuAddress"
                        />
                </div>

                <div class="form-group">
                    <label for="stuEnrollDate">Enrollment-Date:</label>
                    <input
                        type="date"
                        class="form-control"
                        id="stuEnrollDate"
                        placeholder="Enter Enrollment-Date"
                        name="stuEnrollDate"
                        />
                </div>

                <input
                    type="button"
                    class="btn btn-primary"
                    id="empSave"
                    value="Save"
                    onclick="saveData();"
                    />
                <input
                    type="button"
                    class="btn btn-primary"
                    id="empChange"
                    value="Change"
                    onclick="changeData();"
                    />
                <input
                    type="button"
                    class="btn btn-primary"
                    id="empReset"
                    value="Reset"
                    onClick="resetForm()"
                    />
                <!-- window.location.reload() -->
            </form>
        </div>

        <script>
            function validateAndGetFormData() {
                var stuIdVar = $("#stuId").val();
                if (stuIdVar === "") {
                    alert("Student Roll-No Required Value");
                    $("#stuId").focus();
                    return "";
                }
                var stuNameVar = $("#stuName").val();
                if (stuNameVar === "") {
                    alert("Student Name is Required Value");
                    $("#stuName").focus();
                    return "";
                }
                var stuClassVar = $("#stuClass").val();
                if (stuClassVar === "") {
                    alert("Student Class is Required Value");
                    $("#stuClass").focus();
                    return "";
                }

                var stuDOBVar = $("#stuDOB").val();
                if (stuDOBVar === "") {
                    alert("Student Birth-Date is Required Value");
                    $("stuDOB").focus();
                    return "";
                }

                var stuAddressVar = $("#stuAddress").val();
                if (stuAddressVar === "") {
                    alert("Student Address is Required Value");
                    $("#stuAddress").focus();
                    return "";
                }

                var stuEnrollDateVar = $("stuEnrollDate").val();
                if (stuEnrollDateVar === "") {
                    alert("Student Enrollment-Date is Required Value");
                    $("stuEnrollDate").focus();
                    return "";
                }

                var jsonStrObj = {
                    stuId: stuIdVar,
                    stuName: stuNameVar,
                    stuClass: stuClassVar,
                    stuDOB: stuDOBVar,
                    stuAddress: stuAddressVar,
                    stuEnrollDate: stuEnrollDateVar
                };
                return JSON.stringify(jsonStrObj);
            }

            function getstuIdASJsonObj() {
                var stuid = $("#stuid").val();
                var jsonStr = {
                    id: stuid
                };
                return JSON.stringify(jsonStr);
            }

            function getStu() {
                var stuIdJsonObj = getstuIdASJsonObj();
                var getRequest = createGET_BY_KEYRequest(
                        connToken,
                        stuDBName,
                        stuRelationName,
                        stuIdJsonObj
                        );
                jQuery.ajaxSetup({async: false});
                var resJsonObj = executeCommandAtGivenBaseUrl(
                        getRequest,
                        jpdbBaseURL,
                        jpdbURL
                        );
                jQuery.ajaxSetup({async: true});
                if (resJsonObj.status === 400) {
                    $("#save").prop("disabled", flase);
                    $("#reset").prop("disabled", false);
                    $("#stuname").focus();
                } else if (resJsonObj.status === 200) {
                    $("#stuid").prop("disabled", true);
                    fillData(resJsonObj);
                    $("#change").prop("disabled", false);
                    $("#reset").prop("disabled", false);
                    $("#stuname").focus();
                }
            }

            function resetForm() {
                $("#stuId").val("");
                $("#stuName").val("");
                $("#stuClass").val("");
                $("#stuDOB").val("");
                $("#stuAddress").val("");
                $("#stuEnrollDate").val("");
                $("#stuId").focus();
            }

            function changeData() {
                $("#change").prop("disabled", true);
                jsonChg = validateData();
                var updateRequest = createUPDATERecordRequest(
                        ConnToken,
                        jsonChg,
                        stuDBName,
                        stuRelationName,
                        localStorage.getItem("recno")
                        );
                jQuery.ajaxSetup({async: falses});
                var resJsonObj = executeCommandAtGivenBaseUrl(
                        updateRequest.jpdBaseURL,
                        jpdbIML
                        );
                jQuery.ajaxSetup({async: true});
                console.log(resJsonObj);
                resetForm();
                $("#stuID").focus();
            }

            function saveData() {
                // validate form data

                // create JPDB request string - token , dbname , relation name ...

                // Execute this request

                //Reset the form data

                var jsonStr = validateAndGetFormData();
                if (jsonStr === "") {
                    return;
                }
                var putReqStr = createPUTRequest(
                        "90933011|-31949325197712829|90949744",
                        jsonStr,
                        "SCHOOL-DB",
                        "STUDENT-TABLE"
                        );

                alert(putReqStr);

                jQuery.ajaxSetup({async: false});
                var resultObj = executeCommandAtGivenBaseUrl(
                        putReqStr,
                        "http://api.login2explore.com:5577",
                        "/api/iml"
                        );
                jQuery.ajaxSetup({async: true});

                alert(JSON.stringify(resultObj));
                resetForm();
            }
        </script>
    </body>
</html>
![image](https://github.com/207r1a66e5/Micro-Project/assets/130493141/0e870979-2110-4d52-a9e4-6ab4c9591fe1)

