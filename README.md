# File-realPath
This is real path of selected files multiple or single file for file uploading 

  selectFile.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                bottomSheetDialog.dismiss();
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("*/*");
                //intent.putExtra(Intent.EXTRA_ALLOW_MULTIPLE, true);//don't use the line. multiple image selector working not proper.
                intent.addCategory(Intent.CATEGORY_OPENABLE);
                intent.putExtra(Intent.EXTRA_LOCAL_ONLY, true);
                try {
                    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR2) {
                        intent.putExtra(Intent.EXTRA_ALLOW_MULTIPLE, true);
                    }
                    startActivityForResult(Intent.createChooser(intent, getResources().getString(R.string.chooser_title)),FILE_REQUEST_CODE);
                } catch (Exception ex) {
                    System.out.println("browseClick :"+ex);//android.content.ActivityNotFoundException ex
                }
            }
        });
        
        
         @RequiresApi(api = Build.VERSION_CODES.KITKAT)
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
      if (requestCode==FILE_REQUEST_CODE && resultCode==RESULT_OK){
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR2) {
                ArrayList<String> selectedSingleFile=new ArrayList<>();
                FileUtility fileUtility=new FileUtility();
                if (data.getClipData() != null) {//multple upload here..
                    ClipData clipData = data.getClipData();
                    int count = clipData.getItemCount();
                    for (int i = 0; i < count; i++) {
                        String filepath=fileUtility.getUriRealPath(getApplicationContext(),clipData.getItemAt(i).getUri());
                        selectedSingleFile.add(filepath);
                    }
                    uploadTask(selectedSingleFile);//add the list for upload task
                }else{//single file here
                    try {
                        Uri uri = data.getData();
                        String filepath=fileUtility.getUriRealPath(getApplicationContext(),uri);
                        selectedSingleFile.add(filepath);
                        uploadTask(selectedSingleFile);//add the list for upload task
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
    }
