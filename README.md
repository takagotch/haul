### haul
---
https://github.com/vinta/Haul

```py
// tests/test.py

TESTS_DIR = os.path.abspath(os.path.join(__file__, '../'))

class HaulBaseTestCase(unittest.TestCase):

  def setUp(self):
    self.complete_html = read_file(os.path.join(TESTS_DIR, 'fixtures/page.html'))
    
    
class HaulResultTestCase(HaulBaseTestCase):

  def setUp(self):
  
  def test_is_found_true():
  
  def test_is_found_false():
  
class FindImagesFromHTMLTestCase(HaulBaseTestCase):

  def setUp(self):
  
  def test_find_html_document(self):
  
  def test_find_html_fragment(self):

class FindImageFromURLTestCase(HaulBaseTestCase):













class CustomFinderPipelineTestCase(HaulBaseTestCase):

  def setUp(self):
    super(CustomFinderPipelineTestCase, self).setUp()
    
  def test_find_html_document(self):
    from haul.compat import str
    
    def img_data_src_finder(pipeline_index,
        soup,
        finder_image_urls=[],
        *args, **kwargs):
      
      now_finder_image_urls = []
      
      for img in soup.find_all('img'):
        src = img.get('data-src', None)
        if src:
          src = str(src)
          now_finder_image_urls.append(src)
          
      output = {}
      output['finder_image_urls'] = finder_image_urls + now_finder_image_urls
      
      return output
      
    FINDER_PIPELINE = (
      'haul.finders.pipeline.html.img_src_finder',
      'haul.finders.pipeline.html.a_href_finder',
      'haul.finders.pipeline.css.background_image_finder',
      img_data_src_finder,
    )
    
    h = Haul(finder_pipeline=FINDER_PIPELINE)
    hr = h.find_images(self.complete_html)
    
    self.assertIsInstance(hr, HaulResult)
    
    test_image_url = 'http://files.heelsfetishism./media/heels/2013/10/03/18099_307xxx.jpg'
    self.assertIn(test_image_url, hr.finder_image_urls)
    
   image_urls = hr.image_urls
   image_urls_count = len(image_urls)
   self.assertEqual(image_urls_count, 7)

class ExceptionsTestCase(HaulBaseTestCase):
  
  def setUp(self):
    super(ExceptionsTestCase, self).setUp()
  
  def test_invalid_parameter_error(self):
    h = Haul()
    
    with self.assertRaises(exceptions.InvalidParameterError):
      url_or_html = None
      h.find_images(url_or_html)
      
  def test_retrieve_error(self):
    h = Haul()
    
    with self.assertRaises(exceptions.RetrieveError):
      h.find_images(self.not_exist_url)
      
    with self.assertRaises(exceptions.RetrieveError):
      h.find_images(self.broken_url)
      
  def test_content_type_not_supported(self):
    h = Haul()
    
    with self.assertRaises(exceptions.ContentTypeNotSupported):
      h.find_images(self.not_supported_url)

if __name__ == '__main__':
  unittest.main()
```

```
```

```
```

