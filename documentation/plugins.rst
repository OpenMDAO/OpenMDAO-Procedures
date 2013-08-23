OpenMDAO Plugins Docs
=====================

Editing Plugin Docs
---------------------

To change the docs in an existing plugin repo, you should fork the repo, then make a clone,
work on a branch, push up the branch, and then issue a pull request.

Forking requires read+write permission. If you don't have it, ask Ken Moore. 

#.  Go to Plugins organization on GitHub at:  https://github.com/OpenMDAO-Plugins
    
    Go to the desired repo and fork it.
    
    (It's okay for plugins to have different licenses.) 


#.  Clone the repo if it's a new one that you don't already have a clone for. You should be in your work area (e.g.,
    OpenMDAO/dev/pziegfel) when you clone: 

    ::
    
      git clone git@github.com:OpenMDAO-Plugins/ipoptwrapper.git

#.  Then go to the repo and add your personal fork:

    ::
    
      git remote add myfork git@github.com:pziegfeld/ipoptwrapper.git

    Since you are the owner of the remote branch (i.e., pziegfeld), you don't include the "OpenMDAO-Plugins" part in
    the command. However, the origin is OpenMDAO-Plugins/ipoptwrapper.git

    To see your remote branches, type:

    ::

      git remote -v  

    You should get something like this:

    ::

       myfork  git@github.com:pziegfeld/ipoptwrapper.git (fetch)
       myfork  git@github.com:pziegfeld/ipoptwrapper.git (push)
       origin  git@github.com:OpenMDAO-Plugins/ipoptdriver.git (fetch)
       origin  git@github.com:OpenMDAO-Plugins/ipoptdriver.git (push)


#.  If it's a new plugin and you need to install it, you must first activate the environment in your personal clone of the
    OpenMDAO-Framework. Then you can install the plugin as a develop egg. 

    
    Go into the top directory of your plugin repo and type: 

    :: 
    
      plugin install
      
    .. Note::  Just ``plugin install`` is needed--without specifying the plugin name--because you're already in the repo!	


#.  If you want to make any changes, you need to create a branch and make the changes on it.


#.  After you've changed a plugin doc and you want to view it, type:

    :: 
    
      plugin build_docs <yourpluginpath>/<yourpluginname>      
      
                 for example,
     
      plugin build_docs /OpenMDAO/dev/pziegfel/pyopt_driver
      
    
    Sphinx should start running.
      
    .. Note::  I think that you can type just ``plugin build_docs <plugin_name>`` without typing the full path to
               build the docs--at least after the first time you build. 


#.  When you have finished your changes, commit them, and then push up the branch using the following command with the name of
    your branch. Remember, you have to be on the branch you are pushing up.
    
    ::
    
      git push myfork pyopt_driver_edits  

    
9.  Now go to your personal repo on GitHub and issue a pull request.
 

Installing a Plugin from OpenMDAO
----------------------------------

To list your installed plugins, go to the OpenMDAO-Framework and type:
 
  ::
    
    plugin list 

To get a list of available OpenMDAO plugins, type:
 
  ::
    
    plugin list --github
      
You should see a list something like this:      

  ::

     Available plugin distributions
     ==============================

        	    GeoMACH -- Geometry-centric MDAO of Aircraft Configurations with High fidelity
	      adpac_wrapper -- Component wrapper for ADPAC (Advanced Ducted Propfan Analysis Code)
     analysis_server_plugin -- This plugin contains a server that can serve OpenMDAO models to be executed in Modelcenter.
	      dakota_driver -- OpenMDAO driver for DAKOTA (Design Analysis Kit for Optimization and Terascale Applications)
               drea_wrapper -- OpenMDAO component wrapper for DREA (Differential Reduced Ejector/Mixer Analysis)
	      excel_wrapper -- OpenMDAO Excel Wrapper (MS Windows only)
	      flops_wrapper -- Component wrapper for FLOPS
	   hsrnoise_wrapper -- OpenMDAO component wrapper for DREA (Differential Reduced Ejector/Mixer Analysis)
        	ipoptdriver -- Driver wrapper for the IPOPT optimization code
        	 montecarlo -- DOEgenerator for Monte Carlo Simulation
        	 nas_access -- Resource Allocator for NAS
	     nastranwrapper -- Component wrapper for MSC Nastran
        	 neural_net -- A neural net surrogate model generator based on the FFnet library
	   nreltraining2013 -- docs and code for the 2013 nrel training class on OpenMDAO
	     ommodelwrapper -- OpenModelica Model Wrapper
	   overflow_wrapper -- Component wrapper for OVERFLOW (OVERset grid FLOW solver)
        	 pdcyl_comp -- Component wrapper for PDCYL
               pyopt_driver -- Driver wrapper for the open-source optimization package pyOpt
        	vsp_wrapper -- Component wrapper for VSP (Vehicle Sketch Pad)


To install a particular plugin, type:

    ::
     
      plugin install --github <plugin_name>














