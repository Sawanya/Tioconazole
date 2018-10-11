<html>
  <head>
    <script src="js/Three.js"></script>
    <script src="js/kekule/kekule.min.js?module=chemWidget,calculation,io,html,openbabel,indigo"></script>
    <script src="js/openchemlib/openchemlib-full.js"></script>
    <link rel="stylesheet" type="text/css" href="js/kekule/themes/default/kekule.css" />
  </head>
  <body>
    <header id="header"></header>
    <main>
      <div id="chemViewer" style="width:1000px;height:650px"
        data-widget="Kekule.ChemWidget.Viewer3D" data-enable-toolbar="true" data-auto-size="false" data-padding="20"
        data-toolbar-evoke-modes="[0]">
      </div>
    </main>
    <footer></footer>

    <script>
      var chemViewer;

      function drawSmiles(smilesString) {
        var molecule = OCL.Molecule.fromSmiles(smilesString, { 'noCoordinates': true, 'noStereo': false })
        molecule.addMissingChirality();
        var chemObject = Kekule.IO.loadFormatData(molecule.toMolfileV3(), 'mol');
        chemViewer.setChemObj(chemObject);  // 2D

        var chemMolecule = Kekule.ChemStructureUtils.getTotalStructFragment(chemObject);

        var calculator = Kekule.Calculator.generate3D(chemMolecule, {'forceField': ''},
          function(chem3Dobject) {
            chemViewer.setChemObj(chem3Dobject);
          },
          function(err) {
            Kekule.error(err);
          });
      }

      function init() {
        Kekule.Indigo.enable();

        chemViewer = Kekule.Widget.getWidgetById('chemViewer');
        chemViewer.setPredefinedSetting('basic');

        adjustSize();

        window.onresize = adjustSize;

      
        var SodiumChlorideSMILES = '[Na+].[Cl-]';
        var CalciumHydrogenPhosphateSMILES = 'OP(=O)([O-])[O-].[Ca+2]'; 
        var Carbon_TetrachlorideSMILES = 'C(Cl)(Cl)(Cl)Cl';
        var SodiumHydroxideSMILES = '[OH-].[Na+]';
        var SodiumHTrisodium_PhosphateSMILES = '[O-]P(=O)([O-])[O-].[Na+].[Na+].[Na+]';
        var SodiumAcetyleneSMILES = 'C#C';
        var Bicarbonate_IonSMILES = 'C(=O)(O)[O-]';
        var Chlorine_HeptoxideSMILES = 'O=Cl(=O)(=O)OCl(=O)(=O)=O';
        var Methanol_SMILES = 'CO';
        var Tioconazole_SMILES = 'C1=CC(=C(C=C1Cl)Cl)C(CN2C=CN=C2)OCC3=C(SC=C3)Cl';

        
        drawSmiles(Tioconazole_SMILES);
      }

      function adjustSize() {			
        var dim = Kekule.HtmlElementUtils.getViewportDimension(document);
        var headerDim = Kekule.HtmlElementUtils.getElemClientDimension(document.getElementById('header'));
        chemViewer.setWidth(dim.width - 10 + 'px').setHeight(dim.height - 10 - headerDim.height + 'px');
      }

      Kekule.X.domReady(init);
    </script>
  </body>
</html>
