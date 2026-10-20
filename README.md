# Jozi Haven B&B: Static Website

Static HTML/CSS/JS site for the fictional Jozi Haven Bed & Breakfast (Johannesburg), hosted on Amazon S3.
Part of the PRAESIGNIS AWS re/START Special Projects.

## Structure

```
index.html          Home: availability box, rooms, guest services, good to know, location, contact
booking.html        Booking request form with live price estimate
confirmation.html   Shows the submitted request (read from the URL)
error.html          404 page (uses root-relative paths so it renders at any URL depth)
css/style.css       All styling
js/main.js          Mobile menu, date rules, booking estimate, confirmation summary
images/             favicon.svg plus room photos: garden-room.jpg, jacaranda-room.jpg, family-room.jpg
```

Room rates and capacities live in one place: the `ROOMS` object at the top of `js/main.js`.
Keep it in step with the prices in `index.html`.

## Deploy

Replace `BUCKET` and `REGION` (for example `af-south-1`).

1. Create an S3 bucket named `BUCKET`. Under Permissions, turn off "Block all public access".
2. Properties, Static website hosting: enable, index document `index.html`, error document `error.html`.
3. Permissions, Bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicRead",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::BUCKET/*"
  }]
}
```

4. Upload the site (README is excluded):

```
aws s3 sync . s3://BUCKET --exclude "README.md" --exclude ".git/*" --delete
```

5. Open `http://BUCKET.s3-website.REGION.amazonaws.com` and test: home, booking form, confirmation, and a bad URL for the 404 page.

The S3 website endpoint is HTTP only. For HTTPS and a custom domain, put CloudFront in front of the bucket.
