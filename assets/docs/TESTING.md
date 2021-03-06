- [Testing](#testing)
  * [Open the resource page](#open-the-resource-page)
  * [Check API versus Form search](#check-api-versus-form-search)
  * [API Anomaly found](#api-anomaly-found)
  * [Check blank search term works](#check-blank-search-term-works)
  * [Testing enhancement Selection on department name rather than department id](#testing-enhancement-selection-on-department-name-rather-than-department-id)
  * [Check Departments modal form works](#check-departments-modal-form-works)
  * [Check pagination of found works](#check-pagination-of-found-works)
  * [Repeat selection criteria](#repeat-selection-criteria)
  * [Testing enhancement Criteria warning display](#testing-enhancement-criteria-warning-display)
  * [Problem when only one page found.](#problem-when-only-one-page-found)
  * [Testing 2 criteria warnings](#testing-2-criteria-warnings)
  * [Responsiveness on search results](#responsiveness-on-search-results)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Testing

### Open the resource page 
Read introduction informing viewer of its purpose.

![Intro Laptop](../images/project_screenshots/Test_Intro_20201013_laptop.jpg)


### Check API versus Form search
Check that the Metropolitan Museum of Art's API search request produces the same number of objects as the search criteria given by the form.

**Museums search API invoked**
![Museums API search](../images/project_screenshots/Testing_searchAPI.jpg)

**Form's criteria search invoked**
![Forms search](../images/project_screenshots/Testing_searchFORM.jpg)

For the given criteria both give a total of 13 objects found in the Metropolitan Museum of Art's collection.

### API Anomaly found
Unfortunately found that when reversing the order of the criteria in the API call, I received only one object returned.
![Museum API reversed order search](../images/project_screenshots/Testing_searchAPIanomaly.jpg)

Added issue #36 on 7th October to API's Github and given email address: openaccess@metmuseum.org, on September 22nd 2020.
![Issue 36](../images/project_screenshots/API_Issue_36.jpg)

### Check blank search term works

**Museums search API invoked with blank 'q'**

![Museums API blank search](../images/project_screenshots/Test_SearchAPI_dept21_q_blank.jpg)

**Form's criteria search invoked with blank 'q'**

![Forms Search with blank q](../images/project_screenshots/Test_SearchFORM_dept21_q_blank.jpg)

**Museums search API invoked with empty quotes 'q'**

![Museums API q quotes search](../images/project_screenshots/Test_SearchAPI_dept21_q_emptyquotes.jpg)

**Form's search invoked with empty quotes 'q'**

![Forms Search empty quotes q](../images/project_screenshots/Test_SearchFORM_q_emptyquotes.jpg)

_Although both API and Form searches match results, I believe that the returned result for blank search term 'q' to be misleading. 
Proposing to intercept blank search terms on form and replace with empty quotes._

### Testing enhancement Selection on department name rather than department id

Incorporated a drop down selection list of department names rather than numeric department identifiers within the selection criteria form.
Testing that the department name selected can be easily converted back to it's identifier for the API search endpoint.

![Selection form's department name](../images/project_screenshots/Test_Select_Department_20201010.jpg)

![Selection form's department (11)](../images/project_screenshots/Test_Department_selection_20201010.jpg)

![Result of a department name search](../images/project_screenshots/Test_Department_selection_result_20201010.jpg)

Comparing with API's search using same search parameters
![Result of API department id search](../images/project_screenshots/Test_Department_API_search_20201010.jpg)

Result totals match.


### Check Departments modal form works

![Department modal screen](../images/project_screenshots/Test_Dept_%20Mode_20201013_laptop.jpg)

Please note that the API does not return departments of id 2 nor 20.

![Department API](../images/project_screenshots/Test_Dept_%20Mode_20201013_API.jpg)


### Check pagination of found works

Search for more than one page (5 works) of art works.

![Initial Top page](../images/project_screenshots/Test_Pages_%2020201013_top2.jpg)

![Initial botton page](../images/project_screenshots/Test_Pages_%2020201013_bottom2.jpg)

Next page

![Top next page](../images/project_screenshots/Test_Pages_%2020201013_top2_next.jpg)

![Bottom next page](../images/project_screenshots/Test_Pages_%2020201013_bottom2_next.jpg)

Previous page

![Top previous page](../images/project_screenshots/Test_Pages_%2020201013_top2_prev.jpg)

![Bottom previous page](../images/project_screenshots/Test_Pages_%2020201013_bottom2_prev.jpg)

### Repeat selection criteria

Due to first selection returning 0 results, attempted to call the selection form again and amend selection criteria.

This produced an error in the selection criteria form. 

Repeated department name selection drop-down input fields.

![Selection Error](../images/project_screenshots/Test_Selection_20201013_Error.jpg)

**Fix**

```javascript
document.getElementById("selDept").innerHTML += selHTML;
```

changed to:

```javascript
document.getElementById("selDept").innerHTML = selHTML;
```

### Testing enhancement Criteria warning display

Introduced a warning division to display search form validation messages.
This is to capture 'dateBegin' and 'dateEnd' errors, as API endpoint discusses 
> You must use both dateBegin and dateEnd 

![No begin date selected](../images/project_screenshots/Test_Warning_20201014_no_begin_selection.jpg)

![Warning message for no begin date](../images/project_screenshots/Test_Warning_20201014_no_begin.jpg)

No end date selected gives a warning too.

![Warning message for no end date](../images/project_screenshots/Test_Warning_20201014_no_end.jpg)

### Problem when only one page found

When only one page worth of art works are found for a selection, then there should not be a 'next page' button displayed.

![Next page when only one page](../images/project_screenshots/Test_Page1_20201014.jpg)

When this 'next page' button is selected, the works disappear, and there is no button to get them back (without re-entering the selection form).

![Next page4 selected](../images/project_screenshots/Test_Page1_20201014_nexp_page_selected.jpg)


**Fix**

```javascript
function generatePaginationButton(pageCnt) {
   
    document.getElementById("metPages").innerHTML = `<table><tr><td>`;
    if ( pageCnt > 1) {
        document.getElementById("metPages").innerHTML += `<button id="btnPrev1" onClick="writePreviousPage(${pageCnt})" class="btn btn-secondary btn-sm">Previous 5 artworks of <span class="badge badge-light">${pageCnt}</span> pages</button>`;
    }
    document.getElementById("metPages").innerHTML += `</td></tr>`;

    document.getElementById("metPages").innerHTML += `<tr><td>`;
    if ( pageCnt > 1) {
        document.getElementById("metPages").innerHTML += `<button id="btnNext1" onClick="writeNextPage(${pageCnt})" class="btn btn-secondary btn-sm">Next 5 artworks of <span class="badge badge-light">${pageCnt}</span> pages</button>`;
    }
    document.getElementById("metPages").innerHTML += `</td></tr>`;
 ```
### Testing 2 criteria warnings

As well as allowing incomplete dates, but warning of the selection, could have a blank search term 'q'.
Currently only the one warning is displayed.

![No search term or end date](../images/project_screenshots/Test_Warning_20201015_no_q.jpg)

Checked to see if the API search gives the same result. It does.

![No search term nor end date API](../images/project_screenshots/Test_Warning_20201015_no_q_API.jpg)

**Fix**

```javascript
    qryStr = document.getElementById("metArtCriteria").elements.namedItem("queryString").value;
    /*
        Has a query term been given?  
        Warning if blank.
    */
    if ( qryStr == "" ) {
        document.getElementById("metWarnings").innerHTML += `<p><mark>Blank search term </mark></p>`;
    }
```

![Two warnings given](../images/project_screenshots/Test_Warning_20201015_no_q_ok.jpg)

### Responsiveness on search results

**Using [Am I Responsive](http://ami.responsivedesign.is/)**

At first the search results looked OK, but wider viewports could utilise the right hand side for the artwork's details, whilst the left hand side is left for the image(s).

![Found objects responsive](../images/project_screenshots/Testing_responsive_2020-10-07.jpg)

Testing different screen sizes:
| Chrome's Inspect emulator           | width  | breakpoint |
|-------------------------------------|--------|------------|
| Nokia Lumia                         | 320px  | (default)  |
| Nexus 7                             | 600px  | sm         |
| iPad :                              | 768px  | sm/md      |
| Kindle Fire:                        | 800px  | md         |
| iPad Pro:                           | 1024px | lg         |
| Laptop with MDPI screen             | 1280px | xl         |


**Nokia**'s viewport _truncated total line_:

![Intro Nokia](../images/project_screenshots/Test_Intro_20201013_nokia.jpg)

![Nokia found top](../images/project_screenshots/Test_Nokia_found_top_20201016.jpg)

![Nokia found bottom](../images/project_screenshots/Test_Nokia_found_bottom_20201016.jpg)


**Nexus**:

![Intro Nexus](../images/project_screenshots/Test_Intro_20201013_nexus7.jpg)

![Nexus found top](../images/project_screenshots/Test_Nexus_found_top_20201016.jpg)

![Nexus found bottom](../images/project_screenshots/Test_Nexus_found_bottom_20201016.jpg)

**iPad**:

![Intro iPad](../images/project_screenshots/Test_Intro_20201013_iPad.jpg)

![iPad found top](../images/project_screenshots/Test_iPad_found_top_20201016.jpg)

![iPad found bottom](../images/project_screenshots/Test_iPad_found_bottom_20201016.jpg)

**iPad Pro**:

![Intro iPadPro](../images/project_screenshots/Test_Intro_20201013_iPadPro.jpg)

![iPad Pro found top](../images/project_screenshots/Test_iPadPro_found_top_20201016.jpg)

![iPad Pro found top](../images/project_screenshots/Test_iPadPro_found_bottom_20201016.jpg)

**laptop**:

![laptop found top](../images/project_screenshots/Test_laptop_found_top_20201016.jpg)

![laptop found bottom](../images/project_screenshots/Test_laptop_found_bottom_20201016.jpg)