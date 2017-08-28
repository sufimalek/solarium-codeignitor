# solarium-codeignitor
Solarium with code ignitor

# ci-solarium
Solarium package installation in Codeigniter 3.0

# Requirements
	JRE (here using 8)
	Solr (5.4.1)
	Composer 
	Codeigniter 3
	
# Steps
1. Install solarium package using composer
			composer require solarium/solarium
2. Add autoload.php to your Codeigniter’s index.php before require_once BASEPATH.’core/CodeIgniter.php’; 
			include_once './vendor/autoload.php';
3. Create a config file with parameters - application/config/solarium.php

			<?php
					$config['solarium_endpoint'] = array(
					    'endpoint' => array(
					        'localhost' => array(
					            'host' => 'localhost',
					            'port' => 8983,
					            'path' => '/solr/',
					            //'core' => 'collection1'
					        )
					    )
					);
			?>
			
			
4. Make a controller - application/controllers/Test.php

                        <?php
                        defined('BASEPATH') OR exit('No direct script access allowed');
                        
                        /**
                        * Controller for handling search using solarium package 
                        * using composer package in Vendor Folder
                        *
                        * 
                        */
                        
                        class Test extends CI_Controller {
                                public function __construct() {
                        		parent::__construct();
                                        $this->config->load('solarium');
                                }
    
                                /**
                                * Select all the records
                                *
                                * Using solarium packages
                                */
                	        public function index() {
                
                        		$client = new Solarium\Client($this->config->item('solarium_endpoint'));
                        		$query = $client->createSelect();
                        		//$query->setQuery('name:test'); - To perform search
                        		$resultset = $client->select($query);
                        		echo 'NumFound: '.$resultset->getNumFound() . PHP_EOL;
                        
                        
                        		foreach ($resultset as $document) {
                        		    echo '<hr/><table>';
                        		    // the documents are also iterable, to get all fields
                        		    foreach($document AS $field => $value)
                        		    {
                        		        // this converts multivalue fields to a comma-separated string
                        		        if(is_array($value)) $value = implode(', ', $value);
                        		        echo '<tr><th>' . $field . '</th><td>' . $value . '</td></tr>';
                        		    }
                        		    echo '</table>';
                        		}
                	        }
                	}
                        ?>
			
			

