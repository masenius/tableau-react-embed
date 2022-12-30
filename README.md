Note: This project is using a older version of the Tableau Embedding API (v2.0.0, whereas v3.4.X is currently available)

To use a more up-to-date api version you can either:
* Manually download, copy, and export a version of the tableau API (as this repo's "tableau-api" dependency does)
* Use an up-to-date project (such as the [TableauEmbed](https://github.com/stoddabr/react-tableau-embed-live) react component)

# Tableau React Component
Tableau React component integrated with Tableau JS API.

Based on https://github.com/coopermaruyama/tableau-react which is not maintained anymore.

## Install

```
npm install tableau-react-embed --save
```

## Usage
```js
import TableauReport from 'tableau-react-embed';

const SimpleReport = props => (
  <TableauReport
    url="http://reports.my-site.com/my-workbook/my-report"
    token="<TRUSTED TICKET HERE>"
  />
)
```

## Supported props
```js

const options = {
  height: 100,
  width: 100,
  hideTabs: false,
  // All other vizCreate options are supported here, too
  // They are listed here: https://onlinehelp.tableau.com/current/api/js_api/en-us/JavaScriptAPI/js_api_ref.htm#ref_head_9
};

const filters = {
  Colors: ['Blue', 'Red'],
  Sizes: ['Small', 'Medium']
};

const parameters = {
  Param1: 'Value',
  Param2: 'Other Value'
};

// Number of seconds between sending Tableau data refresh requests. 0 = do not automatically refresh (default)
const refreshSeconds = 60 * 5;

const MyReport = props => (
  <TableauReport
    url="http://reports.my-site.com/my-workbook/my-report"
    filters={filters}
    parameters={parameters}
    options={options} // vizCreate options
    refreshSeconds={refreshSeconds}
  />
)
```

### Changing Filters & Parameters

Any parameters or filters that are initially passed will be sent in the
vizCreate options, which speeds up initial loading time by not having to apply
each one asynchronously after the viz loads.

Once the viz has been loaded, if the parameters/filters change but the url
does not, only the changed parameters/filters will be applied asynchronously in
order to optimize performance.

### Automatic Data Refreshing

You can specify the number of seconds to wait before automatically refreshing the data in the Viz using `props.refreshSeconds`. Any number less than 1 will cause the data to not be automatically refreshed. This uses the viz.refreshDataAsync() method in the Tableau JS API.

If you change this prop, the timer will be reset and will start waiting for the new number of seconds before triggering the refresh. It will not immediately refresh, but rather wait until the new timer goes off. It will not stop refreshes in progress.

Warning: The next refresh may be called again before the last refresh completes, so make sure to set refreshSeconds high enough to prevent overlap if you use this feature.


### Viz Integration
Upon initialization, a new Viz will be created. A new Viz will only be
initialized if `props.url` changes for performance reasons.

### Trusted Tickets

You can authenticate using a trusted ticket, which will be immediately
invalidated upon being used, because using it a second time will log the user
out.

If `props.token` gets updated, it will use it the next time a viz is initialized.

## Testing
```
npm install
npm test
```
