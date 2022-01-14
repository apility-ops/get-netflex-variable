# get-netflex-variable

Lets you run requests against the Content api and easilly extract values from a sites settings in order to use them when running your workflows.

## Usage 

```yaml
steps:
    - uses: 'actions/checkout@v2'
    - uses: 'some-action-that-retrieves-site-key'
      id: siteInfo
    
    # This is how you bootstrap a call to get a variable
    - name: Get FontAweesome token 
      id: fontAwesome
      uses: apility-ops/get-netflex-variable@main
      with:
        publicKey: ${{ steps.siteInfo.outputs.publicKey }}
        privateKey: ${{ steps.siteInfo.outputs.privateKey }}
        alias: "font-awesome-key" # This corresponds to a setting alias in Netflex
    

    - name: Set FontAwesome Token
      if: steps.info.outputs.fontAwesomeToken
      run: |
        npm config set "@fortawesome:registry" https://npm.fontawesome.com/
        npm config set "//npm.fontawesome.com/:_authToken" $TOKEN
      env:
        TOKEN: ${{ steps.fontAwesome.outputs.value }} # Notice that the steps.fontAwesome corresponds with the id of the variable step.
```