# notion-backup

![example workflow name](https://github.com/jckleiner/notion-backup/workflows/notion-backup-docker-workflow/badge.svg?branch=master)

### Set Credentials

Create a `.env` file with the following properties ([How do I find all these values?](./documentation/setup.md)):

    # Make sure not to use any quotes around these environment variables
    
    # Notion (Required)
    NOTION_SPACE_ID=
    NOTION_EMAIL=
    NOTION_PASSWORD=

    # Options: markdown, html, csv. Default is markdown.
    NOTION_EXPORT_TYPE=markdown

    # Google (Optional)
    GOOGLE_DRIVE_ROOT_FOLDER_ID=
    GOOGLE_DRIVE_SERVICE_ACCOUNT=
    # Provide either secret json or the path to the secret file
    GOOGLE_DRIVE_SERVICE_ACCOUNT_SECRET_JSON=
    GOOGLE_DRIVE_SERVICE_ACCOUNT_SECRET_FILE_PATH=

    # Dropbox (Optional)
    DROPBOX_ACCESS_TOKEN=

    # Nextcloud (Optional)
    NEXTCLOUD_EMAIL=
    NEXTCLOUD_PASSWORD=
    NEXTCLOUD_WEBDAV_URL=

### Backup to Cloud with Docker

Once you created your `.env` file, you can run the following command to start your backup:

```bash
docker run \
    --rm=true \
    --env-file=.env \
    jckleiner/notion-backup
```

The downloaded Notion export file will be saved to the `/downloads` folder in the Docker container and the container
will be removed after the backup is done (because of the `--rm=true` flag).

### Local Backup with Docker

If you want to keep the downloaded files locally, you could mount the `/downloads` folder from the container somewhere
on your machine:

```bash
docker run \
    --rm=true \
    --env-file=.env \
    -v <backup-dir-absolute-path-on-your-machine>:/downloads \
    jckleiner/notion-backup
```

You could also set a cronjob if you want trigger backups in regular intervals.

### Fork (GitHub Actions)

Another way to do automated backups is using GitHub Actions. You can simply:

1. Fork this repository.
2. Create repository secrets: Go to `notion-backup > Settings > Secrets > Actions` and create all
   the [necessary environment variables](#set-credentials).
3. You can adjust when the action will be triggered by editing your `notion-backup/.github/workflows/docker.yml`
   workflow file.

## Troubleshoot

### Dropbox

If you get the exception: `com.dropbox.core.BadResponseException: Bad JSON: expected object value.`, then try to
re-generate your access token and run the application again.
