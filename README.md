# C# Live Project

## Introduction

The C# Live Project at the Tech Academy is a two week sprint through the TheatreCMS project. The purpose of this project is to work as a team to update a theater/acting company website for users who are not technically saavy and want to easily manage what displays in their website. Since the project started months before I was brought in and therefore many features were already built, during those two weeks I had the opportunity to work on several [front end stories](#front-end-stories).

## Front End Stories

* [Cast Member Delete Page Layout](#cast-member-delete-page-layout)
* [New Message Bar](#new-message-bar)
* [Donation Page and Donation Goal](#donation-page-and-donation-goal)
* [Award Delete Page Buttons](#award-delete-page-buttons)

### Cast Member Delete Page Layout

The view file initially provided me with default html string lines and a default delete button. My task was to change the layout by meticulously positioning elements to match a provided image, including creating and displaying a blue "CurrentMember" pill button if the Cast Member is a current one, enhance the default delete button to match the standard, and create a back to list button. For smaller screen sizes, all information was to be moved below the image.

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

For this Story, my task was to create a messaging area on layout. I was provided an image to replicate which as to be fixed to the bottom-left of the screen. The green dot and the value were to be hardcoded at the time. Here I created a partial view:

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

*Jump to: [Page Top](*live-project), [Front End Stories](*front-end-stories)
