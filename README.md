# getSmallBitmap
根据路径获得图片信息并按比例压缩，并把图片保存到SD卡

     
    public static File getSmallBitmap(String filePath) {
        final BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;//只解析图片边沿，获取宽高
        BitmapFactory.decodeFile(filePath, options);
        // 计算缩放比
         final int height = options.outHeight;
        final int width = options.outWidth;
        int inSampleSize = 1;
        if (height > 800 || width > 480) {
            final int heightRatio = Math.round((float) height / (float) 800);
            final int widthRatio = Math.round((float) width / (float) 480);
            inSampleSize = heightRatio < widthRatio ? heightRatio : widthRatio;
        }  
        // 完整解析图片返回bitmap
        options.inJustDecodeBounds = false;
        Bitmap bitmap= BitmapFactory.decodeFile(filePath, options);
        /////////////////////////////////////////////////////////////////////////////////////
        String oldname=filePath.substring(filePath.lastIndexOf("/"),filePath.lastIndexOf("."));
        String name=oldname+"_cpmpress.jpg";
        String saveDir=Environment.getExternalStorageDirectory()+"/compress";
        File dir=new File(saveDir);
        if(!dir.exists()){
            dir.mkdir();

        }

        File file=new File(saveDir,name);//将要保存图片的路径
        try {
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file));
            bitmap.compress(Bitmap.CompressFormat.JPEG, 100, bos);
            bos.flush();
            bos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        return file;

    }
      
