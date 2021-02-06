# C# Live Project 

## Introduction

The C# Live Project at the Tech Academy is a series of two-week sprints through the TheatreCMS project. The purpose of this project is to work as a team to update a theater/acting company website for users who are not technically saavy and want to easily manage what displays in their website. Since the project started months before I was brought in and therefore many features were already built, during the Live Project I had the opportunity to work on several [front end stories](#front-end-stories) and [back end stories](#back-end-stories).

## Front End Stories

* [Cast Member Delete Page Layout](#cast-member-delete-page-layout)
* [New Message Bar](#new-message-bar)
* [Donation Page and Donation Goal](#donation-page-and-donation-goal)
* [Award Delete Page Buttons](#award-delete-page-buttons)
* [Upgrade Award Index Page](#upgrade-award-index-page)

### Cast Member Delete Page Layout

The view file initially provided me with default html string lines and a default delete button. My task was to change the layout by meticulously positioning elements to match a provided image, including creating and displaying a blue "CurrentMember" pill button if the Cast Member is a current one, enhancing the default delete button to match the standard, and creating a back to list button. For smaller screen sizes, all information was to be moved below the image.

    @using TheatreCMS.Controllers
    @model TheatreCMS.Models.CastMember
    @using TheatreCMS.Models

    @{
        ViewBag.Title = "Delete";
    }

    <div class="castMemDelPage">
        <h3 class="castMemDelTitle">Delete this Cast Member?</h3>
        <div class="container">

            <!-- 1st row -->
            <div class="row">

                <!-- 1st column (twice as large as 2nd column) -->
                <dl class="col-sm-8">
                    <dd>
                        <!-- Display Cast Member Image -->
                        <img class="castImage, castMemDelImage" src="@Url.Action("DisplayPhoto", "Photo", new { id = Model.PhotoId })" />

                        <div>
                            <!-- If current member, display pill button -->
                            @if (Model.CurrentMember)
                            {
                                <button class="castMemDelPillButton">CurrentMember</button>
                            }
                        </div>
                    </dd>
                </dl>

                <!-- 2nd column (half as large as 1st column) -->
                <dl class="col-sm-4">
                    <dt>
                        UserName
                    </dt>

                    <dd class="col-md-9">
                        @ViewBag.CurrentUser
                    </dd>
                    
                    <dt>
                        @Html.DisplayNameFor(model => model.Name)
                    </dt>

                    <dd class="col-md-9">
                        @Html.DisplayFor(model => model.Name)
                    </dd>

                    <dt>
                        @Html.DisplayNameFor(model => model.MainRole)
                    </dt>

                    <dd class="col-md-9">
                        @Html.DisplayFor(model => model.MainRole)
                    </dd>

                    <dt>
                        @Html.DisplayNameFor(model => model.YearJoined)
                    </dt>

                    <dd class="col-md-9">
                        @Html.DisplayFor(model => model.YearJoined)
                    </dd>
                    
                    <dt>
                        @Html.DisplayNameFor(model => model.DebutYear)
                    </dt>

                    <dd class="col-md-9">
                        @Html.DisplayFor(model => model.DebutYear)
                    </dd>

                    <dt>
                        @Html.DisplayNameFor(model => model.CastYearLeft)
                    </dt>

                    <dd class="col-md-9">
                        @Html.DisplayFor(model => model.CastYearLeft)
                    </dd>
                    
                    <!-- AssociateArtist Checkbox -->
                    <dt class="castMemDelCheckCntr">
                        <div>
                            @Html.DisplayFor(model => model.AssociateArtist)
                        </div>

                        <div class="castMemDelCheckTitle">
                            @Html.DisplayNameFor(model => model.AssociateArtist)
                        </div>
                    </dt>

                    <!-- EnsembleMember Checkbox -->
                    <dt class="castMemDelCheckCntr">
                        <div class="mr-1">
                            @Html.DisplayFor(model => model.EnsembleMember)
                        </div>

                        <div class="castMemDelCheckTitle">
                            @Html.DisplayNameFor(model => model.EnsembleMember)
                        </div>
                    </dt>
                </dl>
            </div>

            <!-- 2nd row -->
            <div class="row">
                <dl class="col-sm-12">
                    <dd>
                        @Html.DisplayFor(model => model.Bio)
                    </dd>
                </dl>
            </div>
            <div>
                @using (Html.BeginForm())
                {
                    @Html.AntiForgeryToken()
                    <div class="form-actions no-color">
                        <!-- Back to List button -->
                        <button class="iconBtn castMemDelBackToList" type="button" onclick="window.location.href ='@Url.Action("Index", "CastMembers")'" formmethod="get">
                            <i class="fa fa-hand-point-left fa-fw"></i>Back To List
                        </button>
                        <!-- Delete button -->
                        <button type="submit" class="iconBtn">
                            <i class="fa fa-trash-alt fa-fw"></i>Delete
                        </button>
                    </div>
                }
            </div>
        </div>
    </div>

The following is the related CSS code I created:

    /***************************/
    /* Cast Member Delete Page */

    /* Styling to position page content toward the center */
    .castMemDelPage {
        margin: auto;
        max-width: 640px;
    }

    /* Styling to center title */
    .castMemDelTitle {
        padding-top: 30px;
        text-align: center;
    }

    /* Cast Member image styling */
    .castMemDelImage {
        /* display + margin properties 
        help center image in column: */
        display: block;
        margin-left: auto;
        margin-right: auto;
        max-width: 400px;
        width: 100%;
        height: auto;
        padding-bottom: .4em; /* Create space between image and pill button */
    }

    /* Pill button styling */
    .castMemDelPillButton {
        border: none;
        background-color: #0080ff;
        color: white;
        font-weight: bold;
        pointer-events: none; /* Disables pointer hover effect */
        border-radius: 16px;
    }

    /* Checkmark containers styling to prevent line breaking */
    .castMemDelCheckCntr {
        position: relative;
    }
    .castMemDelCheckTitle {
        position: absolute;
        left: 23px;
        top: 0; /* Position content on same line */
    }

    .castMemDelBackToList {
        margin: 0;
    }

    /* END Cast Member Delete Page */
    /*******************************/

### New Message Bar

For this Story, my task was to create a messaging area on layout. I was provided an image to replicate which was to be fixed to the bottom-left of the screen. The green dot and the value were to be hardcoded at the time. Here I created a partial view:

    <!-- 
        01/01/21
        Partial view for New Message Bar.

        Directly related files:
        ~Controllers/MessageController.cs
        ~Views/Shared/_Layout.cshtml   --(Search "NEW MESSAGE BAR")
        ~Content/Site.css   --(Search "NEW MESSAGE BAR")
    -->

    <div class="newMsgBar">
        <i class="newMsgCommentDotsIcon fas fa-comment-dots"></i>
        <i class="newMsgIndicator fas fa-circle fa-stack"></i>
        <span class="newMsgQuantity">&nbsp;4 </span>
        <span class="newMsgTitle">&nbsp;New Messages</span>
        <i class="newMsgUpIcon fas fa-angle-up"></i>
    </div>

Added code to the controller:

    using System.Web.Mvc;

    // See ~Views/Shared/_Messaging.cshtml

    namespace TheatreCMS.Controllers
    {
        public class MessageController : Controller
        {
            // GET: Message
            public ActionResult Index()
            {
                return View();
            }

            [ChildActionOnly]
            public PartialViewResult GetAddress()
            {
                return PartialView("_address");
            }
        }
    }

Added code to the layout:

    <!-------------------- NEW MESSAGE BAR -------------------->
    <!---------- See ~Views/Shared/_Messaging.cshtml ---------->
    @{Html.RenderPartial("_Messaging");}
    <!--------------------------------------------------------->

And the CSS:

    /* ------------------- NEW MESSAGE BAR ------------------- */
    /*
        See ~Views/Shared/_Messaging.cshtml
    */

    .newMsgBar {
        overflow: hidden;
        cursor: pointer;
        position: fixed;
        bottom: 0;
        left: 0;
        width: 325px;
        background-color: #1a1a1a;
        z-index: 2; /* Must be at least 2, otherwise the Slideshow 
                    on the Home page will be selected instead 
                    when clicked on. */
    }

    /* Icon Styling */
    .newMsgCommentDotsIcon {
        background-color: #121212;
        color: white;
        padding: 6px 8px 6px 8px;
        font-size: 21px;
    }
    .newMsgIndicator {
        position: absolute;
        top: -11px;
        left: -18px;
        color: forestgreen;
        font-size: 13px;
    }
    .newMsgUpIcon {
        position: relative;
        right: -32px;
        background-color: #121212;
        color: white;
        padding: 6px 11px 6px 11px;
        font-size: 21px;
    }

    /* Character Styling */
    .newMsgQuantity {
        color: forestgreen;
        font-weight: bold;
        position: relative;
        left: -35px;
        font-size: 21px;
    }
    .newMsgTitle {
        color: white;
        font-weight: bold;
        position: relative;
        left: -35px;
        font-size: 21px;
    }

    /* ----------------- END NEW MESSAGE BAR ----------------- */

### Award Delete Page Buttons

For this Story, my task was to fix and standardize the buttons on the Award Delete page.

    @using (Html.BeginForm("Delete", "Awards", FormMethod.Post, new { id = "DeleteForm" })) {
        @Html.AntiForgeryToken()
        <div class="form-actions no-color">
            <a class="btn btn-main" onclick="document.getElementById('DeleteForm').submit()"><i class="fa fa-trash-alt fa-fw"></i>Delete</a>
            <a class="btn btn-main" href="/Awards"><i class="fa fa-hand-point-left fa-fw"></i>Back To List</a> 
        <div>
    }

### Donation Page and Donation Goal

For this Story, my task was to create a DonationGoal property and give Administration the ability to view and edit their donation goal. In the Company dropdown in the navbar, I added a "Donate!" link to the Donation\Create page. Then I created the new property in the AdminSettings.cs file under AdminSettings, and made a new field in the Admin Dashboard so the Admin can change the value.

### Upgrade Award Index Page

For this Story, my task was to create code that italicizes a recipient's name and places an information icon next to it if the recipient has an associated Cast Member. Selecting the icon displays a modal containing information on that Cast Member, selecting anywhere outside of the modal closes it, and selecting the photo takes the user to that Cast Member's details page. 

Here I created the Views\Awards\Index.cshtml page:

    @using System.Collections.Generic;
    @model IEnumerable<TheatreCMS.Models.Award>

    @{
      ViewBag.Title = "Awards";
    }

    <div class="awards">
      <h2>Awards</h2>

      <!--light Navbar for Creating & Searching Content-->
      <nav class="navbar navbar-light">
        <div class="navbar-nav mr-auto mt-2 mt-lg-0">
          <button class="iconBtn" onclick="window.location.href ='@Url.Action("Create", "Awards")'">
            <!-- or buttons can be in their own container-->
            <i class="fa fa-plus-square fa-fw"></i>Create New<!-- This button is a link to a different page -->
          </button>
        </div>
        <form class="form-inline my-2 my-lg-0">
          <!--filters table values based on text value-->
          <input class="form-control mr-sm-2" type="text" placeholder="Search" aria-label="Search" id="searchFilter">
          <!-- clears search bar and resets to normal page after the onclick-->
          <button class="iconBtn" type='button' title="Reset Search" id="clearSearch">
            <!--Code below was initially part of above button, separated and commented out to replace with clearSearch function in jQuery-->
            <!--onclick="document.getElementById('Search').value = ''; $('#award-search tr').show(); "-->
            <i class="fas fa-undo"></i> <!--This button refreshes the page-->
          </button>
        </form>
      </nav>

      <table class="table" style="color:white">
        <tr>
          <th>
            @Html.DisplayNameFor(model => model.Production.Title)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.Year)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.Name)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.Type)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.Category)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.Recipient)
          </th>
          <th>
            @Html.DisplayNameFor(model => model.OtherInfo)
          </th>
          <th>Options</th>
        </tr>

        @foreach (var item in Model)
        {
          <tbody id="awardTable">
            <tr>
              <td>
                @Html.DisplayFor(modelItem => item.Production.Title)
              </td>
              <td>
                @Html.DisplayFor(modelItem => item.Year)
              </td>
              <td>
                @Html.DisplayFor(modelItem => item.Name)
              </td>
              <td>
                @Html.DisplayFor(modelItem => item.Type)
              </td>
              <td>
                @Html.DisplayFor(modelItem => item.Category)
              </td>
              <!-- If recipient has an Associated Cast Member, italicize recipient and display icon. -->
              <td>
                @if (item.CastMemberId != null)
                {
                  <span class="font-italic">
                    @Html.DisplayFor(modelItem => item.CastMember.Name)
                  </span>
                  <a data-ToggleButton="@item.AwardId" class="CastMemDetailsButton-Toggle"><i class="fa fa-info-circle fa-fw"></i></a>
                }
                @* Else display recipient normally. *@
                else
                {
                  @Html.DisplayFor(modelItem => item.Recipient)
                }
              </td>


              @****** Modal for Cast Member Details when info button is selected ******@

                  <div data-ToggleButton="@item.AwardId" class="awardCastMemDetails awardCastMemDetails-Popup">
                      @{
                        string img = "";
                        if (item.CastMember != null)
                        {
                          @* When user selects modal photo, send to its respective details page *@
                          img = Url.Action("DisplayPhoto", "Photo", new { id = item.CastMember.PhotoId });
                          <a href="@Url.Action("Details", "CastMembers", new { id = item.CastMember.CastMemberID })"> <img class="card-img-top awardCastMemDetails-Img" @*id="castImage"*@ src="@img" /></a>
                        }
                        else
                        {
                          img = Url.Content("~/Content/Images/CastMember.jpg");
                        }
                      }
                      <h4>
                          @Html.LabelFor(model => item.CastMember.YearJoined)
                      </h4>
                      <p>
                          @Html.DisplayFor(model => item.CastMember.YearJoined)
                      </p>
                      <h4>
                          @Html.LabelFor(model => item.CastMember.CastYearLeft)
                      </h4>
                      <p>
                          @Html.DisplayFor(model => item.CastMember.CastYearLeft)
                      </p>
                      <h4>
                          @Html.LabelFor(model => item.CastMember.DebutYear)
                      </h4>
                      <p>
                          @Html.DisplayFor(model => item.CastMember.DebutYear)
                      </p>
                      <p>
                          @Html.DisplayFor(model => item.CastMember.CurrentMember) @Html.LabelFor(model => item.CastMember.CurrentMember)
                      </p>
                  </div>
              @*********************** END Modal ***********************@
                <td>
                  @Html.DisplayFor(modelItem => item.OtherInfo)
                </td>
                <td>
                  <button class="iconBtn" onclick="window.location.href ='@Url.Action("Edit", "Awards",  new { id = item.AwardId } )'">
                    <!-- or buttons can be in their own container-->
                    <i class="fa fa-edit fa-fw"></i>Edit<!-- This button is a link to a different page -->
                  </button>
                  <button class="iconBtn" onclick="window.location.href ='@Url.Action("Delete", "Awards",  new { id = item.AwardId } )'">
                    <!-- or buttons can be in their own container-->
                    <i class="fa fa-trash-alt fa-fw"></i>Delete<!-- This button is a link to a different page -->
                  </button>
                  <button class="iconBtn" onclick="window.location.href ='@Url.Action("Details", "Awards",  new { id = item.AwardId } )'">
                    <!-- or buttons can be in their own container-->
                    <i class="fa fa-info-circle fa-fw"></i>Details<!-- This button is a link to a different page -->
                  </button>
                </td>
              </tr>
            </tbody>

          }
      </table>
    </div>

    <script>
    @*Filter table values based off text in search bar, refresh button undoes filter.*@
      $(document).ready(function () {
        $("#searchFilter").on("keyup", function () {
          var searchText = $(this).val().toLowerCase();
          $("#awardTable tr").filter(function () {
            $(this).toggle($(this).text().toLowerCase().indexOf(searchText) > -1)
          });
        });
        $('#clearSearch').click(function () {
          $("#searchFilter").val("");
          $("#searchFilter").trigger("keyup")
        });
      });


    @******************jQuery Button Toggle Function***************@

      $(".CastMemDetailsButton-Toggle").click(function () {
          var AwardId = $(this).attr("data-ToggleButton");
          $(".awardCastMemDetails-Popup[data-ToggleButton=" + AwardId + "]").addClass("awardCastMemDetails-VisiblePopup");
      });

      $(document).click(function (event) {
          //If user clicks on anything except the modal itself, remove modal-displaying class
          if (!$(event.target).closest(".awardCastMemDetails-Popup,.CastMemDetailsButton-Toggle").length) {
              $("body").find(".awardCastMemDetails-Popup").removeClass("awardCastMemDetails-VisiblePopup");
          }
      });
    </script>

And the CSS:

     /*************************************/
    /* Award Details Index Page | part 2 */

    /* Popup form hidden by default and centered */
    .awardCastMemDetails-Popup {
        border: 3px solid #f0f1f0;
        z-index: 10;
        display: none;
        position: fixed;
        left: 50%;
        top: 55%;
        transform: translate(-50%, -50%);
        background-color: red;
        color: white;
        font-weight: bold;
        max-width: 300px;
        border-radius: 11px;
    }

    .awardCastMemDetails-Img {
        border-radius: 7px 7px 0px 0px;
    }

    /* jQuery function adds this to 
    awardCastmemDetails-Popup*/
    .awardCastMemDetails-VisiblePopup {
        display: block;
    }

    .awardCastMemDetails-Popup h4, p {
        margin-left: 5%;
    }

     /* END Award Details Index Page | part 2 */
    /*****************************************/

## Back End Stories

* [Change Photo Rotation On Click](#change-photo-rotation-on-click)
* [Connect Seeded Member to Cast Member](#connect-seeded-member-to-cast-member)

### Change Photo Rotation On Click

For this Story, my task was to create a pill badge under each production photo that is only viewable by administration. Selecting this badge will switch the button's text between "Do Not Rotate Images" and "Rotate Images," the button's color between gray and green, and the RotateProductionPhotos property between false and true, respectively, without the need to reload the webpage. 

Here I made my addition to the ProductionsController.cs file:

    // Toggle RotateProductionPhotos property
    public JsonResult RotateProductionPhotos(int id)
    {
        var production = db.Productions.Find(id);
        // If property is true, change to false (and vice versa):
        production.RotateProductionPhotos = !production.RotateProductionPhotos;
        db.Entry(production).State = EntityState.Modified;
        db.SaveChanges();
        return Json(new { success = true });
    }

Created the property in the Production.cs model:

    [Display(Name = "Rotate Production Photos")]
    public bool RotateProductionPhotos { get; set; }

And made my additions to the Productions\Index.cshtml page:

    @* Pill badge displayed when User is Admin. *@
    @if (User.IsInRole("Admin"))
    {
        if (item.RotateProductionPhotos)
        {
            @* When RotateProductionPhotos is true, green "Rotate Images" pill badge *@
            <span class="badge badge-pill badge-success RotateProductionsBadge" RotateProductionsBadge="@item.ProductionId" style="cursor: default">Rotate Images</span>
        }
        else
        {
            @* When RotateProductionPhotos is false, gray "Do Not Rotate Images" pill badge *@
            <span class="badge badge-pill badge-secondary RotateProductionsBadge" RotateProductionsBadge="@item.ProductionId" style="cursor: default">Do Not Rotate Images</span>
        }
    }

    (...)

    // Change Photo Rotation on click without refreshing
    $(".RotateProductionsBadge").click(function () {
        var ProductionId = $(this).attr("RotateProductionsBadge");
        $.ajax({
            url: "/Productions/RotateProductionPhotos",
            data: { "id" : ProductionId },
            type: "POST",
            success: function (id) {
                // Select specific badge id:
                var rotateproductionsbadge = $("span[RotateProductionsBadge=" + ProductionId + "]");
                // If badge has the "badge-success" bootstrap class:
                if (rotateproductionsbadge.hasClass("badge-success")) {
                    // Change color by changing bootstrap class:
                    rotateproductionsbadge.addClass("badge-secondary").removeClass("badge-success");
                    // Change text:
                    rotateproductionsbadge.text("Do Not Rotate Images");
                    console.log("RotateProductionPhotos property = false");
                }
                else if (rotateproductionsbadge.hasClass("badge-secondary")) {
                    rotateproductionsbadge.addClass("badge-success").removeClass("badge-secondary");
                    rotateproductionsbadge.text("Rotate Images");
                    console.log("RotateProductionPhotos property = true");
                }
                else {
                    alert("Error regarding Photo Rotation OnClick function.\n\nRotateProductionsBadge=\"" + ProductionId + "\"");
                    console.log("Error when selecting RotateProductionsBadge=\"" + ProductionId + "\"");
                }
            }
        });
    });

### Connect Seeded Member to Cast Member

For this Story, my task was to dynamically assign a specific Cast Member to a specific Seeded User in the Startup.cs file, ensuring that even if the CastMemberId changes due to, for example, additional Cast Members being seeded before her that she will be connected to the User. 

Here I made my addition to the Startapp.cs file:

    ConnectCastMemToSeededMem();

    (...)

    // Dyanamically assigns the "Adriana Gantzer" cast member to the "God Skrilla" user
    private void ConnectCastMemToSeededMem()
    {
        // Return first entity named "Adriana Gantzer"
        var adrianaGantzer = context.CastMembers
            .Where(name => name.Name == "Adriana Gantzer")
            .First();
        // Return first entity named "God Skrilla"
        var godSkrilla = context.Users
            .Where(name => (name.LastName == "Skrilla") && (name.FirstName == "God"))
            .First();

        adrianaGantzer.CastMemberPersonID = godSkrilla.Id;
        godSkrilla.CastMemberUserID = adrianaGantzer.CastMemberID;

        // Save changes
        context.Entry(adrianaGantzer).State = EntityState.Modified;
        context.SaveChanges();
    }

*Jump to: [Page Top](#introduction), [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories)*
