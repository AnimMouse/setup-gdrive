# Setup gdrive for GitHub Actions
Setup [gdrive](https://github.com/prasmussen/gdrive) (Google Drive CLI Client) on GitHub Actions to use `gdrive`.

This action installs [gdrive](https://github.com/prasmussen/gdrive) for use in actions by installing it on /usr/local/bin.

With gdrive, you can now interact with Google Drive like uploading and downloading files inside GitHub Actions. 

This action requires you to use a Google Service Account with a JSON key.

This action only works on Ubuntu virtual environments as [conditionals](https://github.com/actions/runner/issues/646) does not work on [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action) yet.

## Usage
To use `gdrive`, run this action before `gdrive`.

Always add `--service-account ${{ steps.setup-gdrive.outputs.service_account_file_name }}` on `gdrive` so that the service account will be used, it will fail if you did not add that option.

For `${{ steps.setup-gdrive.outputs.service_account_file_name }}` to work, you must add an `id:` syntax so that outputs will work.

For Google Service Account JSON key located on the repository or downloaded from somewhere (Do not use on public repositories!) place the file path on the `service_account_file_path` input.
```yml
steps:
  - uses: actions/checkout@v2
    
  - name: Setup gdrive
    id: setup-gdrive
    uses: AnimMouse/setup-gdrive@v1
    with:
      service_account_file_path: service-account-example.json
      
  - run: gdrive --service-account ${{ steps.setup-gdrive.outputs.service_account_file_name }} about
```

For public repositories, you can use Secrets to place the JSON key, encode the JSON key in base64 `base64 service-account-example.json` and paste the base64 string to Secrets.

Use a base64 to file converter action so that you can use the base64 encoded JSON key in setup-gdrive.

```yml
steps:
  - uses: actions/checkout@v2
    
  - name: Get Google service account file from secrets
    id: google_service_account
    uses: timheuer/base64-to-file@v1.1
    with:
      fileName: google_service_account.json
      encodedString: ${{ secrets.GOOGLE_SERVICE_ACCOUNT }}
      
  - name: Setup gdrive
    id: setup-gdrive
    uses: AnimMouse/setup-gdrive@v1
    with:
      service_account_file_path: ${{ steps.google_service_account.outputs.filePath }}
      
  - run: gdrive --service-account ${{ steps.setup-gdrive.outputs.service_account_file_name }} about
```