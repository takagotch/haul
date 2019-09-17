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

