# zcl_abap_html_to_pdf_converter
convert html to pdf on abap


*&---------------------------------------------------------------------*\
*& Report Z_HTML_TO_PDF_EXAMLE\
*&---------------------------------------------------------------------*\
*& Autror: Anton Sikidin\
*& Email : Anton.Sikidin@gmail.com\
*&---------------------------------------------------------------------*\
report z_html_to_pdf_examle.

data\
      : lv_pdf type xstring\
      .

* 1) first case\
call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
    iv_html_string = '<html><header><title>This is title</title></header><body>Hello world</body></html>'\
*   iv_html_smw0   =\
*   iv_html_xstring =\
*   iv_zip_smwo    =\
*   iv_zip_xstring =\
*   iv_param       = " default A4 PORTRAIT\
  receiving\
    rv_pdf         = lv_pdf.

*2) second case

data\
      :  lt_mime type table of w3mime\
      .

data(ls_key) = value wwwdatatab(\
 relid = 'MI'\
 objid = 'Z_INVOICE_HTML' ).

call function 'WWWDATA_IMPORT'\
  exporting\
    key    = ls_key\
  tables\
    mime   = lt_mime\
  exceptions\
    others = 1.\
if sy-subrc <> 0.\
  return.\
endif.

call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
*   iv_html_string  =\
    iv_html_smw0 = lt_mime\
*   iv_html_xstring =\
*   iv_zip_smwo  =\
*   iv_zip_xstring  =\
*   iv_param     = " default A4 PORTRAIT\
  receiving\
    rv_pdf       = lv_pdf.

* 3) third case

data\
      : lv_html_xstring type xstring\
      .

call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
*   iv_html_string  =\
*   iv_html_smw0    =\
    iv_html_xstring = lv_html_xstring\
*   iv_zip_smwo     =\
*   iv_zip_xstring  =\
*   iv_param        = " default A4 PORTRAIT\
  receiving\
    rv_pdf          = lv_pdf.

*4)  fourth case

 ls_key  = value wwwdatatab(\
 relid = 'MI'\
 objid = 'Z_INVOICE_ZIP_WITH_IMAGE_AND_STILE' ).

call function 'WWWDATA_IMPORT'\
  exporting\
    key    = ls_key\
  tables\
    mime   = lt_mime\
  exceptions\
    others = 1.\
if sy-subrc <> 0.\
  return.\
endif.

call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
*   iv_html_string  =\
*   iv_html_smw0 =\
*   iv_html_xstring =\
    iv_zip_smwo = lt_mime\
*   iv_zip_xstring  =\
*   iv_param    = " default A4 PORTRAIT\
  receiving\
    rv_pdf      = lv_pdf.

*5) fifth case

data\
      : lv_zip_html_xstring type xstring " zip with css, image, html\
      .

call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
*   iv_html_string =\
*   iv_html_smw0   =\
*   iv_html_xstring =\
*   iv_zip_smwo    =\
    iv_zip_xstring = lv_zip_html_xstring\
*   iv_param       = " default A4 PORTRAIT\
  receiving\
    rv_pdf         = lv_pdf.

*6)   :))  i cant count over 5

data\
      : lv_param type zst_html_to_pdf_param\
      .

lv_param-paper_size = 'A3'.\
lv_param-orientation =  'LANDSCAPE'.

call method zcl_abap_html_to_pdf_convert=>convert\
  exporting\
    iv_html_string = '<html><header><title>This is title</title></header><body>Hello world</body></html>'\
*   iv_html_smw0   =\
*   iv_html_xstring =\
*   iv_zip_smwo    =\
*   iv_zip_xstring =\
    iv_param       = lv_param\
  receiving\
    rv_pdf         = lv_pdf.
