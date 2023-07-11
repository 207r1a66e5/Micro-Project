# Micro-Project
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
