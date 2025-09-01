# AWS S3 File Manager

A full-stack web application that allows users to securely connect to their AWS S3 buckets and manage files through an intuitive interface. Built with React (frontend) and Node.js/Express (backend).

## Features

- 🔐 **Secure AWS S3 Connection**: Connect using your AWS credentials with session-based authentication
- 📤 **File Upload**: Upload files directly to your S3 bucket with presigned URLs
- 📋 **File Listing**: View all uploaded files with download links and file sizes
- 🗑️ **File Deletion**: Delete files from your S3 bucket (UI implementation in progress)
- 🎨 **Modern UI**: Clean, responsive interface built with React
- 🔒 **Session Management**: Secure credential handling using express-session

## Tech Stack

### Frontend
- **React 19** - Modern React with hooks
- **Vite** - Fast build tool and dev server
- **Axios** - HTTP client for API requests
- **GSAP** - Animation library (configured)

### Backend
- **Node.js** with **Express** - REST API server
- **AWS SDK v3** - Latest AWS JavaScript SDK
- **express-session** - Session management
- **CORS** - Cross-origin resource sharing

## Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- AWS account with S3 bucket
- AWS IAM user with appropriate S3 permissions

## AWS Setup

### 1. Create an S3 Bucket
1. Go to AWS S3 Console
2. Create a new bucket
3. Note the bucket name and region

### 2. Create IAM User
1. Go to AWS IAM Console
2. Create a new user for programmatic access
3. Attach the following policy (replace `YOUR_BUCKET_NAME`):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR_BUCKET_NAME",
                "arn:aws:s3:::YOUR_BUCKET_NAME/*"
            ]
        }
    ]
}
```

4. Save the Access Key ID and Secret Access Key

## Installation

### 1. Clone the Repository
```bash
git clone <repository-url>
cd aws-s3-file-manager
```

### 2. Install Backend Dependencies
```bash
cd server
npm install
```

### 3. Install Frontend Dependencies
```bash
cd ../client
npm install
```

## Running the Application

### 1. Start the Backend Server
```bash
cd server
npm start
# Server will run on http://localhost:8000
```

### 2. Start the Frontend Development Server
```bash
cd client
npm run dev
# Client will run on http://localhost:5173
```

## Usage

### 1. Connect to S3
1. Open the application in your browser
2. Fill in the connection form:
   - **Bucket Name**: Your S3 bucket name
   - **AWS Region**: Your bucket's region (e.g., `us-east-1`, `ap-south-1`)
   - **Access Key ID**: Your IAM user's access key
   - **Secret Access Key**: Your IAM user's secret key
3. Click "Connect to S3"

### 2. Upload Files
1. After successful connection, you'll see the file manager
2. Click "Choose File" to select a file
3. Click "Upload to S3" to upload the file
4. Files are stored in the `uploads/user-uploads/` prefix in your bucket

### 3. Manage Files
1. View all uploaded files in the file list
2. Click on file names to download
3. See file sizes in KB
4. Use delete buttons to remove files (backend ready, UI in progress)

## API Endpoints

### POST /api/connect
Connect to AWS S3 with credentials
- **Body**: `{ accessKeyId, secretAccessKey, region, bucketName }`
- **Response**: Connection status

### GET /api/generate-upload-url
Generate presigned URL for file upload
- **Query**: `fileName`, `contentType`
- **Response**: `{ uploadUrl }`

### GET /api/list-files
List all files in the bucket
- **Response**: Array of file objects with URLs

### GET /api/delete-url
Generate delete URL for file removal
- **Query**: `key`
- **Response**: Deletion status

## Project Structure

```
aws-s3-file-manager/
├── client/                 # React frontend
│   ├── public/
│   ├── src/
│   │   ├── App.jsx        # Main application component
│   │   ├── App.css        # Styles
│   │   └── main.jsx       # React entry point
│   ├── package.json
│   └── vite.config.js
├── server/                 # Express backend
│   ├── server.js          # Main server file
│   └── package.json
└── README.md
```

## Security Features

- ✅ **No Credential Storage**: AWS credentials are stored only in server sessions
- ✅ **Presigned URLs**: Secure file uploads without exposing credentials to frontend
- ✅ **Session-based Auth**: Connection state managed server-side
- ✅ **CORS Protection**: Configured for specific origin
- ✅ **Input Validation**: Server-side validation of all inputs

## Environment Variables

While the current implementation uses hardcoded values, you can enhance security by using environment variables:

Create `.env` in the server directory:
```env
PORT=8000
SESSION_SECRET=your-session-secret-here
CLIENT_URL=http://localhost:5173
```

## Development

### Frontend Development
```bash
cd client
npm run dev      # Start development server
npm run build    # Build for production
npm run lint     # Run ESLint
```

### Backend Development
```bash
cd server
npm start        # Start server
```

## Troubleshooting

### Common Issues

1. **Connection Failed**
   - Verify AWS credentials are correct
   - Check bucket name and region
   - Ensure IAM user has proper S3 permissions

2. **Upload Failed**
   - Check file size limits
   - Verify bucket permissions
   - Check network connectivity

3. **CORS Errors**
   - Ensure backend is running on port 8000
   - Frontend should be on port 5173

### Debug Mode
Enable detailed logging by checking browser console and server logs.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License.

## Future Enhancements

- [ ] Complete delete functionality in UI
- [ ] File preview for images
- [ ] Drag & drop upload
- [ ] Progress bars for uploads
- [ ] Folder organization
- [ ] Batch operations
- [ ] File sharing capabilities
- [ ] Environment variable configuration
- [ ] Docker containerization

## Support

For issues and questions:
1. Check the troubleshooting section
2. Review AWS S3 documentation
3. Open an issue in the repository

---

**Note**: Never commit AWS credentials to version control. Always use environment variables or AWS IAM roles in production.