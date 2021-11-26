# Setup gdrive for GitHub Actions
Setup [gdrive](https://github.com/prasmussen/gdrive) (Google Drive CLI Client) on GitHub Actions to use `gdrive`.

This action installs [gdrive](https://github.com/prasmussen/gdrive) for use in actions by installing it on `/usr/local/bin/`.

With gdrive, you can now interact with Google Drive like uploading and downloading files inside GitHub Actions. 

This action requires you to use a Google Service Account with a JSON key encoded in base64.

Other virtual environments besides Ubuntu are not supported yet.

## Usage
To use `gdrive`, run this action before `gdrive`.

Always add `--service-account gdrive.json` on `gdrive` so that the service account will be used, it will fail if you did not add that option.

Encode the JSON key in base64 using this command `base64 -w 0 service-account-example.json` and paste the base64 string to Secrets.

```yml
steps:
  - uses: actions/checkout@v2
    
  - name: Setup gdrive
    uses: AnimMouse/setup-gdrive@v2
    with:
      service_account: ${{ secrets.GOOGLE_SERVICE_ACCOUNT }}
      
  - run: gdrive --service-account gdrive.json about
```